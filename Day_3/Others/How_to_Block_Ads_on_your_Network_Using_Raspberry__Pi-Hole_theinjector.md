# How to Block Ads on your Network Using Raspberry & Pi-Hole?

![Logo](https://i.imgur.com/3ZlymTb.png)

## What is a Raspberry Pi?

Raspberry Pi is the name of a series of single-board computers made by the Raspberry Pi Foundation, a UK charity that aims to educate people in computing and create easier access to computing education.
![Logo](https://i.imgur.com/NcJeJI8.jpg)

The Raspberry Pi launched in 2012, and there have been several iterations and variations released since then. The original Pi had a single-core 700MHz CPU and just 256MB RAM, and the latest model has a quad-core CPU clocking in at over 1.5GHz, and 4GB RAM. The price point for Raspberry Pi has always been under $100 (usually around $35 USD), most notably the Pi Zero, which costs just $5.
## What is Pi-hole?
- Pi-hole is a Linux network-level advertisement and Internet tracker blocking application which acts as a DNS sinkhole and optionally a DHCP server, intended for use on a private network

# What do most people use Raspberry Pi for?
All over the world, people use the Raspberry Pi to learn programming skills, build hardware projects, do home automation, implement Kubernetes clusters and Edge computing, and even use them in industrial applications.

- And in this article where going to use Raspberry Pi to block ads on our network
## Requirements Needed hardware
![Logo](https://lukesingham.com/content/images/2020/04/raspberry-pi-zero-w-annotated-2.jpg)

- Raspberry Pi
- SD Card with at least 2GB of free space
- USB power cable
- Ethernet cable

## Raspberry Pi
![Logo](https://i.imgur.com/NcJeJI8.jpg)

## SD-CARD
![Logo](https://i.imgur.com/Q4bDci8.jpg)

## USB Power Cable
![Logo](https://i.imgur.com/iAaeHFf.jpg)

## Ethernet Cable
![Logo](https://i.imgur.com/8LFt6an.jpg)

## Requirements Needed Software
- Raspbian(Raspbian is a free operating system based on Debian optimized for the Raspberry Pi hardware.)
- Pi-Hole
- You don't need a recent Raspberry Pi model.


# Installation
# Step 1
We need to install Raspbian, the default and official Operating System for Raspberry.

Open https://www.raspberrypi.org/software/operating-systems/ and download the Raspberry Pi OS Lite image, we don't need all the fancy stuff like desktop.

- Use Etcher to flash your image.
- Open it and select the .zip file of the Raspbian image you have just downloaded.
- Select the SD card and click on Flash!.
# Step 2
- Configure SSH
- We will need SSH to connect it over the WiFi. It's disabled by default for security reasons.

To enable it, we simply need to create an empty file called ssh (without extension) at the root of the SD Card.

```$ touch ssh```
# Step 3

- Configure WiFi
- Create a file at the root of the SD Card called ```wpa_supplicant.conf ``` and paste this in:
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
country=MA
update_config=1

network={
  ssid="SSID WIFI"
  psk="PASSWORD WIFI"
}
```

- Be careful, it only works with 2.4GHz, not 5GHz.

# Step 4

- Connect to the Raspberry PI
- use angry ip scanner or advanced scanner to find the IP of Raspberry pi
- Open your router settings and assign a static IP address in the DHCP options for the Raspberry PI because we will use it in our DNS settings.

```$ ssh pi@<rpi-ip-address>```
-  Enter ```pi``` as your username and ```raspberry``` as your password. You may want to change these later.

# Step 5
- Upgrade your system
```
$ sudo apt update
$ sudo apt upgrade
```
# Step 6
- Install Pi-hole with this command:

```
$ curl -sSL https://install.pi-hole.net | bash
```

- The Pi-hole installer will start by updating the available software, and then a menu based installation wizard will start. Press Enter to progress through the installation.
- Choose wlan0 as the interface to use with Pi-hole. Press Tab to move the red highlight to Ok and then press Enter.
- Select your upstream DNS provider. We chose Google, but there are many others to choose from. Press Tab and then Enter.

![Logo](https://cdn.mos.cms.futurecdn.net/BFhFKaPiGwypqqnfErZwaa-1200-80.png)

- Accept the default list of blocked sites by pressing tab and enter.


![Logo](https://cdn.mos.cms.futurecdn.net/6T7KkmSLAMbSPdfYirL9Fa-1200-80.png)

- Accept the default IPv4 and IPv6 protocols

![Logo](https://cdn.mos.cms.futurecdn.net/B2vja33tUk73wNWiS6fhqZ-1200-80.png)

- Accept the current network settings, and set them as static

![Logo](https://cdn.mos.cms.futurecdn.net/Q3utKRY3aMCK8a9CCW6H9Z-1200-80.png)

- Install the web admin interface

![Logo](https://cdn.mos.cms.futurecdn.net/5W7cCpvaLqC5bqqc4u3JAa-1200-80.png)

- Install the lightppd web server used to serve the web admin pages
![Logo](https://cdn.mos.cms.futurecdn.net/s7kwg2suiqLqtpcZaKecZb-1200-80.png)

-  Accept the default log options.
- Accept the default privacy mode
- The installation is complete and the final page recaps the IP address of the Pi-hole device and provides an admin webpage login password.

![Logo](https://cdn.mos.cms.futurecdn.net/83Xj5mHSBb2fr6XULVPHnY-1200-80.png)



Change the web admin password in the terminal using the following command.
```
$ pihole -a -p
```

Find the dashboard at this address: http://<rpi-ip-address>/admin/

![Logo](https://dbushell.com/images/blog/2020/pihole-dashboard.png)

# Step 7
- Change your DNS settings in your router
- Tap the name of your router in google and search how to change the dns in your Router.
- Change the dns ip with IP of Raspberry.
# NB
if you want to change the default password of SSH
```
pi@raspberrypi:~ $ sudo raspi-config
```
- change the user password from raspberry to ```newpassword```


Enjoy
## By Theinjector
