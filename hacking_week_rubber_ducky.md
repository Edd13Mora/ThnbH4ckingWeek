hello guys hope y'all doing great i am fadl a 15 years old pentester i'd like to talk to u guys about one of my experiences in "Hardware hacking" let's get started first of all i had the idea of making this tool by watching the famous "Mr Robot" series in one of it's episods a USB was used to hack computers in less than 15 seconds and at this point my dream to make this gadget were born i started looking for a micro controler to use one of the commun ones are the ARDUINO'S , i decided to use the Arduino pro micro known as the Arduino leonardo because it can hide it self inside any operating system because it acts like a trusted "KEYBOARD" when we plug it into the victims computer it starts injecting keystroaks at speed of 100 word/s .


The material used:
Arduino pro micro
computer
micro usb to usb adapter

Software needed :
Arduino IDE

the 2.0 version include some more advanced sttufs like the ability of using more than 16 script at the same time the scripts are defined via a dip switch containing 4 slots , we can mannage each script running via a binary mode : 0000 01000 etc 

Bulding Steps : 

first of all we must connect our arduino with the sd card module
 
 GND --> GND
 VCC --> 5v
 D15 --> SCK
 D14 --> MISO
 D16 --> MOSI
 D4  --> CS
 
 now all we need id to upload our arduino c script to our devlopment board via usb cable 
 by the following steps 
 ------------------
 open arduino ide softwar
 ------------------
 select your arduino board type
 ------------------
 select port
 ------------------
 upload the script and you are Done enjoy your own arduino rubber ducky 
