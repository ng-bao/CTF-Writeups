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

And after overlook all regular folder, we dive into the `program` file. First, we checked the `Roaming` folder then found the `chatapp` database
