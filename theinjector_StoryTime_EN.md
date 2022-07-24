# How did I hack my friend's surveillance camera?And Prank him !

- Hello i'm theinjector, first of all I want to thank all members of The Hacker news B'Darija who provided this project and for all the efforts they put in.
## STORY TIME
- ⚠️ Before starting this story I would like to tell you that I took permission from my friend.

One day, a friend of mine called me during Ramadan for breakfast. Without mentioning the name of the city, I took my bag and went to them with another friend,When we arrived, my attention was drawn to a surveillance camera near his window, and I did not see any cable next to it. The first question that came to my mind was, is this a wireless camera? and 100% contains a memory card What video is recorded in the memory card.

![Logo](https://i.imgur.com/ueDOHbE.jpg)


And I was in the car, I took my computer and did a scan of the WiFi, and it was difficult for me to know the name of the WiFi for it, and I had a doubt whether it was really connected to the WiFi or was it just recorded inside the memory card.  
I turned off the computer and got out of the car and greeted that friend, then we ate and went out at night. When we came back, I started to complete my work. I settled here again. It was easy for me to know the name of the WiFi because I was next to it.
i opened waircut and i tried to hack the WiFi but it didn't show me the netowrk key.It only showed me the Pin code.

After I opened the jumpstart program and i put the pin code and the WiFi connected Successfully.

Then i made a scan on the network with nmap tool
```
nmap -sC -sV -Pn 192.168.1.0/24
```
and I found this IP 192.168.1.103 with almost 7 open ports.


```
PORT     STATE SERVICE
23/tcp   open  telnet
80/tcp   open  http
554/tcp  open  rtsp
843/tcp  open  unknown
3201/tcp open  unknown
5050/tcp open  mmcc
8001/tcp open  vcom-tunnel
```
The first thing after the scan was finished on the network, I took the IP and put it in the browser, I found a white index page, and I increased port 8001. The same thing, I searched for the name of the service that appeared with Nmap. I found a WebUI in some old versions that did not work with me. I searched again and found that you can see a screenshot of the camera live by adding the word snapshot behind the port.
The link will become like this
```
http://192.168.1.103:8001/snapshot
````

This link works without a username and without a password, and every time I refresh the link, the image moves, meaning that it is working. I did a more in-depth search and found a list of the user name and password for the telnet protocol

```
username:admin
password:12345
--------------
username:admin
password:cxlinux
```
I tried many usernames and passwords, but no result.

![Logo](https://i.imgur.com/jaykVj7.png)

I searched the Rtsp protocol > :is an application-level network communication system that transfers real-time data from multimedia to an endpoint device by communicating directly with the server streaming the data.

I opened the vlc program and I went to open media and network and put the link only changed the http protocol with rtsp.

![Logo](https://i.imgur.com/to8Iv0A.png?1)

```
rtsp://192.168.1.103:8001
```
the result after i clicked on open

![Logo](https://i.imgur.com/bFdHOAx.png)

As you have seen, the camera was successfully opened. After i open the router control panel, I found the default username and password admin admin.

![Logo](https://i.imgur.com/OJc06OQ.png)

 and i assigned a static IP with Mac Address of Camera.

 After several times of searching, I found Channel hd and other hd with sound av and av1 I went back to the VLC program and added av1 after opening it. I hear some guys talking near the door of the house and i hear the car doors outside closing by their owners and I watch everything that happens.

After that, I found a root username&password, which you can do anything like flashing the memory of the camera, making a reset for the camera, deleting the conf files, modifying it,download it ...

And I found some cameras of this type that enter the root without the password
```
Camera TV-288ZD-2MP with firmware 3.4.2.0919
CVE-2020-6852
```
```
telnet 192.168.1.103
```
The Login credentials is :

```
username:root
password:cxlinux
```


BOOOM!!!

When I entered the root user, i started laughing and thinking about a prank of my friend & i searched a little bit in the files and folders of the camera.

i found a file called
```wpa_supplicant.conf```

wpa_supplicant is a cross-platform supplicant with support for WPA, WPA2 and WPA3 (IEEE 802.11i). It is suitable for desktops, laptops and embedded systems. It is the IEEE 802.1X/WPA component that is used in the client stations. It implements key negotiation with a WPA authenticator and it controls the roaming and IEEE 802.11 authentication/association of the wireless driver.

and this file allows quickly connecting to a network whose SSID is already known,

![Logo](https://i.imgur.com/eG4wgN6.png)

 after i found inside the mnt folder a folder called mmc01 and inside it another folder called 0 containing folders with the name of the date and time of the recorded videos and here I added a point . to all folders in the beginning until  It is hidden inside the memory card
![Logo](https://i.imgur.com/wibBuFe.png)

And when I saw my friend sleeping, I took his car key and changed its place behind the house.  



![Logo](https://i.imgur.com/LOVM5C7.png)


Early in the morning he found me I had not slept yet I was staying up all night and he took the car key and went down to his car and he didn't find it ,i watched his reaction through the window  he came back home in a state of despair because of his fear for the car then went to the camera and took the memory card and installed it in his phone, he didn't know how to reveal hidden folders because i hid them, so he started to become confused and afraid, so I told him the story and his feelings were confused, and after this event he became passionate about this field and became a fan of it.




## Issues found
The Telnet service is exploited by a brute-force password attack i.e (login: root pass: cxlinux) – CVE-2020-6852
Weak authentication of the RTSP service has been uncovered (no password required) – CVE-2020-9349

## Conclusion:
As the research shows, there are some serious security issues with these devices and its web interface. It is mandatory to fix these issues to improve the security before they get into the wrong hands. Here are some recommendations from our side:

- Close the telnet service as the open telnet is vulnerable to brute-force attacks.
- Fix weak authenticated RTSP access.
- Establish a strong password to avoid attackers getting access to live video feed.
- Sending data to a random website needs to be blocked.
