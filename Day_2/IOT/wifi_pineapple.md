Who am I ?

Hello guys it’s me again I’m fadl known as “FDX07” a 15 years old pentester and hardware hacker .

What is the Wifi pineapple ?



The wifi pineapple is a router developed by Hak5 company , Basically the Wifi pineapple can sniff all the data from any device being connected into it .

What is the TP-LINK MR3020 ?

the TP-LINK MR3020 is a router wich has the ability of acting like an access point and being OpenWrt flashed .

Hardware we need :

TP-Link TL-MR3020 ver 1

1. USB Flash Drive (8GB or more)

Software needed :

OpenWRT

1. WiFi Pineapple Mark V ver.2.2.0

First of all we need to download the OpenWRT firmware by typing this command

cd ~/Desktop

wget [http://downloads.openwrt.org/attitude_adjustment/12.09-rc1/ar71xx/generic/openwrt-ar71xx-generic-tl-mr3020-v1-squashfs-factory.bin](https://downloads.openwrt.org/attitude_adjustment/12.09-rc1/ar71xx/generic/openwrt-ar71xx-generic-tl-mr3020-v1-squashfs-factory.bin)

Step 2 , We must configure our ip adresse

IP address : 192.168.0.10

Gateway    : 192.168.0.1

Then Go to the "System Tools" -> "Firmware Upgrade" -> "Browse" and select the OpenWRT .bin file that you have downloaded. Then click "Upgrade" button to perform upgrade.

Wait a moment as it will upgrade the firmware and rebooting.

Now We must connect our tp-link to a computer via ethernet cable and connect to it on terminal with ssh by typing this command “ssh <root@192.168.1.100>”

The last step is to Start your engine!

Insert back the USB Pendrive to MR3020. Switch "On" MR3020 until it is booted up.

On your laptop, down this code:


wget <http://www.wifipineapple.com/wp5.sh>





THE END THANKS








