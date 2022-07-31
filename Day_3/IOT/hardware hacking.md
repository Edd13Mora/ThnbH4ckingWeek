# Hardware Hacking - Firmware Analysis  by FDX07
<img src="https://pbs.twimg.com/media/FLLNygXWUAAQHfC.jpg" >

# Todays Topic ?

Hello guys , today imma talk to you about how can we extract firmware files , edit them or change them and how can we repload them back into a device , FDX07 is here let's get started
# What is Hardware hacking ?
<img src="https://c.tenor.com/E3SGDcyydMMAAAAC/batman-hmm.gif" height="500px" weight="500px">

**Hardware hacking**is basically a method used by Red Team hackers to break the security of a device making it able to do stuffs we wants , seems to be easy ?
# What do we need as a hardware to get hands on hardware hacking ?
- A computer 

<img src="https://www.gmelectronic.com/data/product/1024_1024/pctdetail.775-275.1.jpg" height="500px" weight="500px">

- Serial converter (UART reader)


- Multimetre

# Our target :
<img src="https://www.ahlaninformatique.ma/wp-content/uploads/2021/07/p_1_1_4_4_1144-TP-Link-Routeur-ADSL2-WiFi-N-300-Mbps.jpg" height="500px" weight="500px">

> FBI mli chafo hdchi :

<img src="https://i.pinimg.com/originals/f6/b1/3c/f6b13ce59168ccf8bf189446f4204a96.jpg" height="500px" weight="500px">

# let's get started boi

First of all we must define the UART pins on router they are like this as default 
**VCC** stands for positive polarity ; **GND** is ground **TX and RX are communication ports**

<img src="https://re-ws.pl/wp-content/uploads/2017/09/pinout.jpg" height="500px" weight="500px">

Connecting them is simple you can connect it to the UART converter as shown in this picture 

<img src="https://www.riverloopsecurity.com/img/blog/hw-101-jtag/goodfet.jpg" height="500px" weight="500px">

# Software part 

after connecting our target with the serial converter let's start listening to packets being injected by the TX and RX port using binwalk after getting all the file on of them was attracting called loginpannel.bin after extracting it from the router i used a bin converter , they are online and free to use , after reading the components of the bin file a .html file were among them using Sublime text editor i realised that it was the login html file soooo i put my website on it now if we connect to the router and type it's ip adress it shows my website 

### For more informations about harware hacking you can visite this video on youtube 

https://www.youtube.com/watch?v=YD6ODeER8qM

> The end i hope u loved this article guys !