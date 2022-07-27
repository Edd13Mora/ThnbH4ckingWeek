# Introduction 

Before starting forgive me on my level of English, so i coincidentally started in the security world, before i liked networking. one day I was reading a course "Maîtrisez vos applications et réseaux TCP/IP" by Eric Lalitte that I recommend to everyone who wants to start in network security, a chapter was about a network attack by Kevin Mitnick that I think everyone knows him.
In this article I will tell you about this attack, which allowed me to go deeper into the world of security, but before I will start with some basic notion in network which will allow you to better understand the attack.

### The OSI model:
The OSI (Open Systems Interconnection) model is a standard that recommends how computers should communicate with each other, or describes seven layers that computer systems use to communicate over a network.
```
But what are these layers?
```
The OSI model is a layered model. This means that it is divided into several pieces called layers, each of which has a defined role.
![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSW0rFwZq8nzZtkrQG04TVl4nnZF20Y1lq_jA&usqp=CAU)
But the internet model is not based on the osi model, it is just the theoretical model, the model that is really used is the tcp/ip model which only has 4 layers.
Each layer has a specific role, for example layer 2 helps machines to communicate in a local network, layer 3 helps machines to communicate between network and The role of layer 4 is  to make applications communicate together.
To make two applications communicate you need a language in the network the language to use is called a protocol and for layer 4, it uses the tcp protocol to make the applications communicate with each other.
### The TCP protocol:

TCP is a layer 4 protocol or also called transport layer protocol which is reliable and which is in connected mode.
```
what does it mean in connected mode?
```
Before communicating, we ensure communication.
Let's take a phone conversation for exemple. Before telling his story, the interlocutor will first make sure that his partner is present at the end of the line. This gives:

- Edd13Mora calls: Tut... tut...
- Akna responds: Alo?
- Edd13Mora begins his conversation: Alo, salam bekher!

We see very clearly here that we must establish communication before talking about the subject. It will be the same in TCP.
```
how is communication establishment done in tcp protocol?
```
The first packet will be a synchronization request, like the tut...tut on the phone, the corresponding flag is the SYN flag (synchronization!).if I want to connect to a server application that works with TCP, I will send a packet with the SYN flag to tell it that I want to communicate with it. A server receiving a SYN request should normally reply that it is OK to communicate with the client. For this it will send an ACK in response (acknowledgment), it's the equivalent of an "Alo". But, it will ask if the client wants to communicate with it and also set the SYN flag in its response. There will therefore be the SYN and ACK flags set in its response.
When we want to communicate in TCP, we do not establish one, but two connections. Because TCP considers that there will be a communication in one direction, and a communication in the other direction. It therefore establishes a connection for each direction of communication.
![](https://snabaynetworking.com/wp-content/uploads/2019/10/TCP-3-Way-Handshake-Process-1.jpg)
Establishing the TCP connection is called the "Three Way Handshake".
what I explained before is just what is happening in a simple way, now let's see in depth what is happening.
![](https://user.oc-static.com/upload/2021/12/01/16383652866638_P2C2-1_1.png)
We see here that the client sends a first segment with the SYN flag set. Its sequence number is then 0 and its acknowledgment number is also 0. Indeed, the sequence number represents the first data byte of the current segment, but since it does not contain any, this number is 0 For the acknowledgment number, it's a bit different. Since this depends on the data sent by the server, and that it has not yet spoken to us, we automatically set it to 0.

Then, the server responds to the client, confirming that it wants to communicate (ACK) and that it also wants to open a communication channel to the client (SYN). Its sequence number is 0, for the same reason as the client. On the other hand, its acknowledgment number is 1. The acknowledgment number is increased by 1 for any response to a segment where the SYN flag is set.
in real life, the first sequence number is not really 0, it is a random value, it has a name, ISN or Initial Sequence Number.
Now we have everything we need to understand the attack.
### Le blind IP spoofing
The idea is that Mitnick must therefore pretend to be the machine with the address @IP A if he wants to be able to connect to the server (the machine with @IP A is authorized to access the server).
For him (Mitnick) to initialize his connection, he must send a SYN pretending to be machine A.
The server will receive a SYN segment from the only machine authorized to speak to it, so it will respond with a SYN+ACK segment.
It only remains for Mitnick to send a segment with ACK to finalize the three way handshake!
```
But it is not that simple !!!
```
There are issues that we haven't seen. Let's look at this in detail.
#### First issue
The server responds to machine A by putting its own sequence number, and that, Mitnick cannot know, since the segment went to machine A, this is in particular why the attack is called blind spoofing, we do not know the sequence number sent by the server, we are blind.

#### Second issue
If Mitnick will send its first SYN to the server. Machine A, which hasn't asked anyone, receives a SYN + ACK segment from nowhere. Machine A will respond with an RST segment to close the connection with the server.
```
So what is Mitnick going to do to fix these two problems and make the attack?
```
#### Solution for the first problem
##### The ISN vulnerability
ISN was dependent on a counter continuously calculated by a computer. Basically, when the computer started, a counter started at 0 and RFC 793 which defines TCP said that this counter should be incremented by 1 every 4 microseconds. Manufacturers who wanted a simpler solution incremented this counter by 128,000 every second, which is not quite equivalent, but of the same order of magnitude.
In addition, this counter was also increased by 64,000 or 128,000 for each TCP connection that was created.
Thus, when a TCP connection started or a machine received a first SYN and had to respond to it by indicating its ISN, it would look at the value of the counter at that instant and take this value as ISN.
```
So the ISN was not entirely random!!!
```
#### Solution for the second problem
##### The SYN flood
if we just send a SYN, the server in front will allocate resources to wait for our response until a timeout is exceeded. If we then send a multitude of SYN packets, the server will allocate a lot of resources on standby to respond to us, while our response will never come. If we send enough SYN to saturate the server's resources, our SYN flood will be successful and the server will no longer be able to respond to new connection requests.
```
 So the connection will not be closed!!
```
#### Complete execution of the attack 
Mitnick launches his SYN flood attack towards machine A in order to make it unreachable on the network and prevent it from responding afterwards. Mitnick just sends a few SYN segments to the server to see how much they are increased. It will then be able to read in the response SYN+ACK segments the value of the sequence number at the time when the server will have sent them. And it will realize that the increase in server ISN is 128,000 between each connection. Mitnick repeats the operation in the same second to know the difference between the sequence numbers of the server. connection opening and will logically respond with a SYN + ACK segment to machine A, having chosen an ISN which will be increased by 128,000 compared to the previous request which arrived in the same second with the program. all that remains is to send back the ACK segment with the correct sequence number, still pretending to be machine A. The server having received a segment with a valid correct acknowledgment number.
![](https://media-exp1.licdn.com/dms/image/C4D22AQGDKouJo6vH4Q/feedshare-shrink_800/0/1638927551060?e=1661385600&v=beta&t=In8981t1uA5OO3G6jXhrxNULXSBLC04o-zJ_dqe78GA)
```
So the connection is done!!
```
Mitnick can send only one command because he will no longer be able to predict the next acknowledgment numbers.
It was therefore enough for Mitnick to send the command:
```
echo ++ >/.rhosts
```
This puts ++ in the .rhosts file. This amounts to saying that any machine has the right to connect to the server, whatever its IP address.
```
So Mitnick gave himself full access to the server from any IP address.
```
##### you have just seen a beautiful and very complex attack, but do not hope for a second to be able to put it into practice. Or you will have to go back in time, in the 90s.