# Hardware Hacking - Firmware Analysis  by FDX07
![Logo](https://pbs.twimg.com/media/FLLNygXWUAAQHfC.jpg)

# Todays Topic ?

Hello guys , today imma talk to you about how can we extract firmware files , edit them or change them and how can we repload them back into a device , FDX07 is here let's get started
# What is Hardware hacking ?
![Logo](https://c.tenor.com/E3SGDcyydMMAAAAC/batman-hmm.gif)
**Hardware hacking** is basically a method used by Red Team hackers to break the security of a device making it able to do stuffs we wants , seems to be easy ?

# What is Firmware?

Firmware is programming that's written to a hardware device's non-volatile memory. Non-volatile memory is a form of static random access memory where the content is saved when a hardware device is turned off or loses its external power source.

Firmware is installed directly onto a piece of hardware during manufacturing. It is used to run user programs on the device and can be thought of as the software that enables hardware to run.



# Types of firmware
There are many types of technology-specific firmware, but all firmware can generally be sorted into three categories based on the level of hardware integration.

- Low-level firmware. Low-level firmware is considered an intrinsic part of a device's hardware. It is often stored on non-volatile, read-only chips like ROM and therefore cannot be rewritten or updated. Devices containing low-level firmware have one-time programmable memory.
- High-level firmware. High level firmware does allow updates and is generally more complex than low-level firmware. In a computer, high-level firmware resides on flash memory chips.
- Subsystem firmware. Subsystem firmware often comes as part of an embedded system. It is comparable to high-level firmware in that it can be updated and is more complex than low-level firmware. One example is a server's power subsystem, which is a piece of server hardware that functions semi-independently from the server.

# Applications of firmware

Firmware is found in a range of computing equipment, including complex devices and those typically not considered computing devices. Some real-world applications of firmware include the following:

- Personal computer. The firmware of a personal computer -- either BIOS or unified extensible firmware interface -- comes embedded on a small memory chip on the computer's motherboard. A computer's peripherals, such as graphics and video cards, also contain firmware.
- Storage devices. USB drives, hard drives and other portable storage devices contain basic firmware that enable them to function with a computer.
- Mobile devices. Mobile phones, tablets, laptops and other mobile devices all contain firmware that let the hardware work with various software.
- Automotive. Automobiles contain many embedded systems, sensors and small computers that contain firmware that enables them to perform their designated tasks.
- Home appliances. Dishwashers and washing machines are among the appliances that contain firmware. The firmware helps the machine communicate with the computer used to configure the machine's settings and control its operation.
- Smart cards. Smart cards have instructions embedded in a chip that provides the card's basic functionality, as well as authentication and encryption.

# What do we need as a hardware to get hands on hardware hacking ?
- A computer
- Serial converter (UART reader)
![Logo](https://www.gmelectronic.com/data/product/1024_1024/pctdetail.775-275.1.jpg)

- Multimetre

# Our target : TD-W8961N
![Logo](https://www.ahlaninformatique.ma/wp-content/uploads/2021/07/p_1_1_4_4_1144-TP-Link-Routeur-ADSL2-WiFi-N-300-Mbps.jpg)

# FBI mli chafo hdchi :
![Logo](https://i.pinimg.com/originals/f6/b1/3c/f6b13ce59168ccf8bf189446f4204a96.jpg)

# let's get started boi

First of all we must define the UART pins on router they are like this as default
**VCC** stands for positive voltage supply ; **GND** is ground **TX and RX are communication pins** TX for is used to send data to our device via serial port and RX is used to receive the data being transmited by our target with a 9600 signal level

![Logo](https://re-ws.pl/wp-content/uploads/2017/09/pinout.jpg)
Connecting them is simple you can connect it to the UART converter as shown in this picture

![Logo](https://www.riverloopsecurity.com/img/blog/hw-101-jtag/goodfet.jpg)
# Software part

After connecting our target with the serial reader, i started listening and interacting with the shell and bootloader. After dumping the firmware, i used binwalk to extract the files.  One of them was attracting called loginpannel.bin. I used a bin converter , they are online and free to use , after reading the components of the bin file a .html file were among them using Sublime text editor i realised that it was the login html file soooo i put my website on it now if we connect to the router and type it's ip adress it shows my website

### For more informations about harware hacking you can visite this video on youtube

https://www.youtube.com/watch?v=YD6ODeER8qM

# The end i hope u loved this article guys !
