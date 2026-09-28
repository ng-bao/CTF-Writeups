# Description
A 17-year-old girl named A left home for unclear
reasons. After being unable to contact her for a while,
her family reported the case to the authorities. You are
given an image extracted from A's computer to look for
traces that can identify A's current location.

Determine the most likely current room, hotel, and
province/city. The final flag is recovered from the
evidence image itself.

# Recommended tools
* FTK image
* DBbrowser for sqlite
* Code editor and necessary environments.

# Solve
We were given a window envidence image.

<img width="248" height="248" alt="image" src="https://github.com/user-attachments/assets/8da1013f-8d41-412d-a85a-f6605efd372b" />

First, we checked directory `Document` then only found some A's information including to-do list and scholarship information.

<img width="604" height="212" alt="image" src="https://github.com/user-attachments/assets/7e21c566-ac78-4039-8195-7f14c42cba18" />

<img width="702" height="236" alt="image" src="https://github.com/user-attachments/assets/3e29baef-f6bd-4e4c-bc60-83ed90e24a87" />

Next to `Downloads`.

<img width="596" height="104" alt="image" src="https://github.com/user-attachments/assets/67ff9a50-94fc-4336-81df-c612637ade5a" />


We can find a booking hotel receipt and a booking image that seems to removed so we only know the receipt which at `html` format.

```html
<!doctype html>
<html><head><meta charset="utf-8"><title>Reservation receipt</title></head>
<body>
<h1>Reservation confirmed</h1>
<p>Guest initial: A</p>
<p>Property: Babarian Hotel</p>
<p>Room: 403</p>
<p>Province: Da Nang</p>
<p>Status shown on this receipt: CONFIRMED</p>
<p>Receipt generated: 2026-08-20 06:56:02 +07.</p>
<p>Reservation code: BAB-403DN</p>
</body></html>

```
But based on the name of two files, we think that they were not the same hotel. Beside that, we found a `notification ` like this.

<img width="428" height="611" alt="image" src="https://github.com/user-attachments/assets/e557602d-8fa8-4e46-a8eb-905944875b23" />

We don't know why the police have to work with A via interne instead offline, it maybe a sign of scam. About the `ticket_DN1842.png`, we even can't open it.

And after overlook all regular folder, we dive into the `program` file.

<img width="110" height="92" alt="image" src="https://github.com/user-attachments/assets/7234f86f-4e2a-469a-bc62-c9b04dad0eac" />

We saw `Chatapp` in `Roaming` and got it database.

<img width="544" height="77" alt="image" src="https://github.com/user-attachments/assets/b3d6849c-e1c1-496d-b637-5c0eb84fcaee" />

By open that database, we can see the conversation between A with `tổ điều tra tài chính` aka `fi-operator-73`.

<img width="281" height="104" alt="image" src="https://github.com/user-attachments/assets/73efcdc6-9ccd-43d4-899f-654edbaa5e9f" />

But the whole conversation between them were encrypted by `AES-256-CBC` so we need to decrypt them. Look through `Local`, we found a function at `Appdata\local\Programs\resources\app.bundle.js` that is used to decrypt them.
```javascript
const crypto = require("crypto");
const ChatAppCrypto = {
  kid: "chatapp-web-v2",
  deriveKey: function (peer, caseId) {
    return crypto
      .createHash("sha256")
      .update([this.kid, peer, caseId].join("|"), "utf8")
      .digest();
  },
  decryptBody: function (body, peer, caseId) {
    const m = JSON.parse(body);
    if (m.alg !== "AES-256-CBC") throw new Error("unsupported");
    const d = crypto.createDecipheriv(
      "aes-256-cbc",
      this.deriveKey(peer, caseId),
      Buffer.from(m.iv, "base64"),
    );
    return Buffer.concat([
      d.update(Buffer.from(m.ct, "base64")),
      d.final(),
    ]).toString("utf8");
  },
};
module.exports = ChatAppCrypto;
```
We put it onto file named `ChatAppCrypto.js`, then export the database at `json` format. But following the parameters, we need to find `peer` and `caseID`. We all knew the one who chat with A is `fi-operator-73` so `peer = fi-operator-73`, next to `caseID`, we can find it in the notification which found in `Downloads`.

<img width="395" height="547" alt="image" src="https://github.com/user-attachments/assets/e9813b22-f521-42e3-a121-5f060b3a2dab" />

After having all parameters, we used AI to write a decrypte code with module `ChatAppCrypto`.
```javascript
const fs = require("fs");
const ChatAppCrypto = require("./ChatAppCrypto.js");

const peer = "fi-operator-73";
const caseId = "FI-217";

const rawData = fs.readFileSync("messages.json", "utf8");
const messages = JSON.parse(rawData);

messages.forEach((item) => {
  const decrypted = ChatAppCrypto.decryptBody(item.body, peer, caseId);
  const directionStr = item.direction === "in" ? "[ĐẾN]" : "[ĐI]";
  console.log(`ID ${item.id} ${directionStr}: ${decrypted}`);
});
```
Result:

<img width="769" height="217" alt="image" src="https://github.com/user-attachments/assets/98797bec-55d9-4d86-b6a5-db367b2b0575" />

By reading the result, we knew why that girl left home for unclear reason. A was threatened by someone claiming to be from `Tổ công tác điều tra`. Following our guest, the scammers initialy leveraged A's scholarship information to construct a fake profile to threaten her. She was subsequently lured into working remotely and communicating through a covert channel (the ChatApp application we recently decrypted). According to the conversation logs in ChatApp, she was instructed to keep this matter confidential and strictly follow all orders. It appears she was ordered to travel to a specific location and permanently wipe all transport and booking ticket records. Additionally, a proof photo was found at the end, which had been XORed with a matching key string. 

But the scammers didn't say A need to delete the browser, it means the cache including all things A downloaded still there so we decided to find the browser`s database and cache to find more envidences. Overmore, 
