# Description
During your shift as a tier-2 SOC analyst, you receive an escalation from a tier-1 analyst regarding a public-facing server. This server has been flagged for making outbound connections to multiple suspicious IPs. In response, you initiate the standard incident response protocol, which includes isolating the server from the network to prevent potential lateral movement or data exfiltration and obtaining a packet capture from the NSM utility for analysis. Your task is to analyze the pcap and assess for signs of malicious activity.

# Tools
* Wireshark
* Browser

# Overview
* This lab is talking about a critical vulnerability in Apache ActiveMQ.
* Announced on Oct. 25, the flaw scored a maximum of 10.0 on the CVSS v3 scale.

# Solve
### Question 1: By identifying the C2 IP, we can block traffic to and from this IP, helping to contain the breach and prevent further data exfiltration or command execution. Can you provide the IP of the C2 server that communicated with our server?
First we can see in packets 1-3, the 3-way handshake from `146.190.21.92` to `134.209.197.3`. Next, we can see the machine `134.209.197.3` sent a `WireFormatInfo` packet over the `OpenWire` protocol to negotiate the data transmission settings.

<img width="853" height="56" alt="image" src="https://github.com/user-attachments/assets/3162241d-c0c0-4952-a1b5-beca59c84bb7" />

And in `packet 6`, the machine `146.190.21.92` sent back an `ExceptionResponse` which was marked as `Malformed Packet`.

<img width="448" height="113" alt="image" src="https://github.com/user-attachments/assets/0f437f4b-6878-417d-ac5d-0888222d3508" />

Answer: `146.190.21.92`

### Question 2: Initial entry points are critical to trace the attack vector back. What is the port number of the service the adversary exploited?
Continuing from the question 1, we can see the attacker C2 server connected to port 61616 to send a `Malformed Packet`. 

Answer: `61616`

### Question 3: Following up on the previous question, what is the name of the service found to be vulnerable?
Diving into 2 packets 4 and 5

<img width="560" height="46" alt="image" src="https://github.com/user-attachments/assets/343d6fc9-c170-4ff4-a4b8-b383c2305d33" />

We can see a string `ActiveMQ` which is the name of the service that was attacked. By searching on the internet, we can find full name of it.
> Apache ActiveMQ® is the most popular open source, multi-protocol, Java-based message broker. It supports industry standard protocols so users get the benefits of client choices across a broad range of languages and platforms. Connect from clients written in JavaScript, C, C++, Python, .Net, and more. Integrate your multi-platform applications using the ubiquitous AMQP protocol. Exchange messages between your web applications using STOMP over websockets. Manage your IoT devices using MQTT. Support your existing JMS infrastructure and beyond. ActiveMQ offers the power and flexibility to support any messaging use-case.

Answer: `Apache ActiveMQ`

### Question 4:The attacker's infrastructure often involves multiple components. What is the IP of the second C2 server?
As we can see in the image at question 3, the attacker forced the ActiveMQ server to initialize an object of a class from the Spring Framework library (ClassPathXmlApplicationContext). This class can read and execute XML configuration files. And in this case that is `invoice.xml`.

<img width="442" height="281" alt="image" src="https://github.com/user-attachments/assets/0c50ee54-4dea-4893-975e-a9ed0979510a" />

By Analyzing it, we can see it downloaded an executable named `docker` from the machine `128.199.52.72` and run it.

<img width="433" height="146" alt="image" src="https://github.com/user-attachments/assets/89dba1be-4aca-4120-a3ac-62293216b206" />

<img width="542" height="136" alt="image" src="https://github.com/user-attachments/assets/5ffd2ecb-5749-4592-a175-0cd5387d9604" />

Answer: `128.199.52.72`

### Question 5: Attackers usually leave traces on the disk. What is the name of the reverse shell executable dropped on the server?
We already found it in question 4.

Answer: `docker`

### Question 6: What Java class was invoked by the XML file to run the exploit?
Looking again at the `xml` file, focus on this line

<img width="436" height="149" alt="image" src="https://github.com/user-attachments/assets/a677ca84-9027-47e9-85e4-b182e8fa0320" />

The attacker initialized class `java.lang.ProcessBuilder` which is used to create and execute processes or commands as OS level. In this case, this class help the attacker download that `docker` file and execute it.

Answer: `java.lang.ProcessBuilder`

### Question 7: To better understand the specific security flaw exploited, can you identify the CVE identifier associated with this vulnerability?
Just searching on the internet with keyword `Apache ActiveMQ ClassPathXmlApplicationContext vulnerability`. We found some document talk about this vulnerability and CVE identifier.

<img width="443" height="418" alt="image" src="https://github.com/user-attachments/assets/ef808a11-f851-4021-af94-3501f43d207a" />

Answer:  `CVE-2023-46604`

### Question 8: The vendor addressed the vulnerability by adding a validation step to ensure that only valid Throwable classes can be instantiated, preventing exploitation. In which Java class and method was this validation step added?
With the keyword at question 7, we can also find a blog talk about how to fix this critical vulnerability from authors: `Siddartha Malladi and Arpit Kataria`. In this blog, they explain the detail of this vulnerability.

The blog said that the main issue causing this vulnerability is the system's failure to properly check the creation of throwable classes within `ExceptionResponseMarshaller`. So the attacker leverages the `createThrowable` method (which can invoke a String-argument constructor on any arbitrary class) to force the system to instantiate Spring Framework's `ClassPathXmlApplicationContext`—even though this class is not a Throwable at all. Subsequently, ClassPathXmlApplicationContext loads a remote XML file from the attacker's server to execute system commands.

And the solution is adding the `validateIsThrowable` method to the `BaseDataStreamMarshaller` parent class so that `ExceptionResponseMarshaller` is forced to validate the data type before instantiating any object from an error packet.
```java
public static void validateIsThrowable(Class<?> clazz) {

    if (!Throwable.class.isAssignableFrom(clazz)) {

        throw new IllegalArgumentException("Class " + clazz + " is not assignable to Throwable");

    }

}
```
First we submited the answer is `BaseDataStreamMarshaller.validateIsThrowable` but it did not match the expected answer format so we tried to find the `BaseDataStreamMarshaller` source code on github because we saw that the attacker used the `createThrowable` method to attack and in the blog have mentioned to it like this
> But the real standout feature is the createThrowable method within the BaseDataStreamMarshaller class. This method has an exceptional ability: it can activate the constructor of any class that requires a String parameter. This innovative function serves as a backbone for strengthening the product's security by ensuring the controlled creation of classes.

So maybe this actually is true method which is added a validation step. After found that class source code, we can find the `createThrowable` method.
```java
private Throwable createThrowable(String className, String message) {
        try {
            Class clazz = Class.forName(className, false, BaseDataStreamMarshaller.class.getClassLoader());
            OpenWireUtil.validateIsThrowable(clazz);
            Constructor constructor = clazz.getConstructor(new Class[] {String.class});
            return (Throwable)constructor.newInstance(new Object[] {message});
        } catch (IllegalArgumentException e) {
            return e;
        } catch (Throwable e) {
            return new Throwable(className + ": " + message);
        }
    }
```
And we have this line: `OpenWireUtil.validateIsThrowable(clazz);`. By checking in commits history, we confirm that line is added at the patch for this vulnerability.

<img width="685" height="335" alt="image" src="https://github.com/user-attachments/assets/a20df0a7-5088-43e2-a985-5c9fb1ce0afa" />

<img width="653" height="140" alt="image" src="https://github.com/user-attachments/assets/3dbe8e75-f29a-4ec0-b85f-616bc10ae7d1" />

Answer: `BaseDataStreamMarshaller.createThrowable`
