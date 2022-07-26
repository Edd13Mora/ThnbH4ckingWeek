# Radio Waves Hacking


Hi, i'm **Uncle J4ck**, for today i will talk to you about an interesting topic called radio-waves-hacking, yes as you can see we will discuss it today and i will use [Fritzing](https://fritzing.org/) to demonstrate the radio wave hacking experience, also i will use arduino UNO to send some modulation to try to decode it using GNUradio and GQRX.


## Composants


In the real world experience, you will need these parts 
- Arduino UNO
<p align="center">
  <img src="https://technologieservices.fr/media/catalog/product/p/l/platine_uno_arduino_275601_1_fb33.jpg?optimize=high&bg-color=255,255,255&fit=bounds&height=700&width=700&canvas=700:700"/>
</p>
- RFM12B
<p align="center">
  <img src="https://cdn.sos.sk/imagecache/product-detail/f6/d5/3119617d/rfm12b-868-s2p-1.jpg"/>
</p>
- RTL-SDR (i tried to remplace it with an android application because i didn't have the device, i broke it XD)
<p align="center">
  <img src="https://www.passion-radio.fr/2301-large_default/rtlsdr-tcxo.jpg"/>
</p>

- Wires (you know them like simple wires what do you expect XD !) 



## Explanation


Before starting out the experience let's explain the process of this experience :

1. Building different types of modulation using Arduino UNO.
2. Sending data over a 868MHz transmitter
3. Capture the traffic with RTL-SDR (or MagicSDR if you don't have the device)
4. Process it using GNURadio and Audacity to decode the modulation and retrieve the originally transmitted data.

> Some basics stuff before get into the real shit 

```
FM vs AM 

I know you heard it a lot but you were too lazy to research it.
```
<p align="center">
  <img src="https://i.imgflip.com/3h742m.jpg"/>
</p>

```
basically FM (Frequency Modulation) and AM (Amplitude Modulation), but the thing is in the frequency you can see the that in FM (while the data transmited as a signal there are only three properties: Amplitude Modulation / Frequency Modulation / Phase Modulation while this 2 properties are constant the only one who's moving is the frequency modulation) and the AM (the other 2 are constant but the amplitude curve is moving). FM it's faster and can transmit 15 times as much informations better than AM, and there's also a different in the frequency architecture  
```
> For the phase modulation i guess not it's important so i will not explain it 

<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/a/a4/Amfm3-en-de.gif?20081207031551"/>
</p>

> You be like after knowing the difference between FM and AM
<p align="center">
  <img src="https://c.tenor.com/IBSVmJACp-0AAAAd/dance-wave-serious-face.gif"/>
</p>

## Softwares

> Btw i'm using a Linux distribution, you can also download these softwares in Windows or any other OS

- [Arduino IDE](https://www.arduino.cc/en/Guide/Linux) 
- [GQRX](http://gqrx.dk/download/install-ubuntu)
- [GNURadio](https://wiki.gnuradio.org/index.php/InstallingGR)
- [Inspectrum](https://github.com/miek/inspectrum/wiki/Build)

#### Steps

> THIS IS IMPORTANT IN CASE YOU SKIPPED YOUR PHYSICS CLASSES XD, GOT YOU BOI 


1. Modulation

> Firstly, let's explain what's a modulation

Modulation, basically it's the conversion of data into radio waves by using the addition of information into an electronic signals, there 2 two major categories of modulation:

- Analog Modulation refers to the process of transferring an analog baseband (low frequency) signal, to create a modulation we use three properties:

1. Amplitude Modulation
2. Frequency Modulation
3. Phase Modulation

> i'm not going to explain them in full details, because there's no puporse at all to explain them XD but not the case for the digital modulation you will see + i already mentioned them in the AM vs FM stuff

  
- Digital Modulation is similar to an analog modulation, except the signal has only two levels either high (logic 1) or low (logic 0), to create a modulation we use three properties:

1. ASK or Amplitude Shift Key / On and off Keying
2. FSK or Frequency Shift Key
3. PSK or Phase Shift Key

> I will explain the first one because it's important for our experience

- ON/OFF keying is a the most used technique to transmit digital-data for a large number of low-frequency RF application, it transmits a large amplitude carrier when it wants to send a `1` and small amplitude carrier when it wants to send a `0` in its simplest form, basically the presence of a carrier for a specific duration represents a binary one, while its absence for the same duration represents a binary zero. it's simple right XD 
<p align="center">
  <img src="https://media.giphy.com/media/xT5LMWOExnRmXt2vFS/giphy.gif
"/>
</p>


2. Sending data

We will use this digital modulation to send some data using a [RFM12B Transmittor](https://www.hoperf.com/data/upload/portal/20190306/RFM12B%20Datasheet.pdf) and an [Arduino-UNO](https://docs.arduino.cc/hardware/uno-rev3) 

> that's the connection part using Fritzing

<p align="center">
  <img src="https://i.imgur.com/kNEKUzj.png"/>
</p>

- I used a LED to detect if we successfully sent a message (it helps in the debugging phase)

```C++
#include <JeeLib.h>

MilliTimer sendTimer;
byte sendSize;
char payload[] = "salam salam salam labass hobala hobala boo faza3a";

void setup () {
    Serial.begin(SERIAL_BAUD);
    Serial.println("\n[Send]");
    rf12_initialize(2, RF12_868MHZ, 33); // initialization of the transmiter
    rf12_encrypt(RF12_EEPROM_EKEY); // encryption data
    pinMode(4, OUTPUT); // LED
}

void loop () {
    rf12_recvDone();
    if (rf12_canSend() && sendTimer.poll(3000)) {
        digitalWrite(4, HIGH);
        // send out a new packet every 3 seconds
        Serial.print("  send ");
        Serial.println((int) sendSize);
        // send as broadcast the payload will be encrypted
        rf12_sendStart(0, payload, sendSize + 4);
        sendSize = (sendSize + 1) % 11;
    } else {
        digitalWrite(4, LOW); 
    }
}
```


```if you want to send and receive using only arduino and the transmiter (no radio waves intercepeting), you can use this code to receive data using the same setting (NodeID / NetworkID ..) but you need to modify the connection and adding a LED for receiving```

```C++
#include <JeeLib.h>

byte recvCount;

// you need to reconfigure the connection part because in the above image, i didn't add a LED for receiving data because we will intercept it 

void setup () {
    Serial.begin(SERIAL_BAUD);
    Serial.println("\n[Recv]");
    rf12_initialize(2, RF12_868MHZ, 33); // initialization iof 
    rf12_encrypt(RF12_EEPROM_EKEY);
}

// this test turns encryption on or off after every 10 received packets

void loop () {
    if (rf12_recvDone() && rf12_crc == 0) {
        // good packet received
        if (recvCount < 10)
            Serial.print(' ');
        Serial.print((int) recvCount);
        // report whether incoming was treated as encoded
        Serial.print(recvCount < 10 ? " (enc)" : "      ");
        Serial.print(" seq ");
        Serial.print(rf12_seq);
        Serial.print(" =");
        for (byte i = 0; i < rf12_len; ++i) {
            Serial.print(' ');
            Serial.print(rf12_data[i], HEX);
        }
        Serial.println();

        recvCount = (recvCount + 1) % 20;
        // set encryption for receiving (0..9 encrypted, 10..19 plaintext)
        rf12_encrypt(recvCount < 10 ? RF12_EEPROM_EKEY : 0);
    }
}
```
> save the files and upload them using arduino-ino (choose your device and the port and upload them)

1. Intercepting

- And for this one you will need a device 
[RTL-SDR](https://osmocom.org/projects/rtl-sdr/wiki/Rtl-sdr) to capture traffics. briefly to explain what's a RTL-SDR, it's a computer based radio scanner for receiving live radio signals, it could receives frequencies from 24 MHZ up to 1766 MHZ. 

> We will be using GNU-Radio-Companion and GQRX
```
GQRX basically used to detect the frequency and in a simple way GRC is used to record the signal and convert it into a wav file (another mp3 extension for sounds) and it creates for you a diagram to have the full overview about the intercept and all the details.
```

we need to install GQRX
```
sudo apt-get install gqrx-sdr
```
Once done, plug the SDR device and type
```
lsusb
```
> As you can see in the image below, you will see the device as in the image below

<p align="center">
  <img src="https://i.imgur.com/h56k15h.png"/>
</p>


- GQRX Configuration:

1. type gqrx and configure I/O.
2. set the frequency at 868 MHZ
3. start capturing traffics 

> That's the interface of GQRX
 <p align="center">
  <img src="https://i.imgur.com/Ww0oQfP.png"/>
</p>

> also you can use [MagicSDR](https://magicsdr.com/) and set the frequency and all the same settings

We need to install GNU-Radio companion 
```
sudo apt-get install gnuradio
```

> This is an example of a graph
<p align="center">
  <img src="https://i.imgur.com/tLGjaHx.png"/>
</p>


1. Decoding 

Because i broke the device, i couldn't capture it and decode the signal, i tried using many Wifi Adapter and trying to somehow change their behaviors (basically re-soldering the little chipset of the TP-LINK and trying random stuff) but it didn't work, it only detect random signals in general so MISSION FAILED 

> but i will put the steps to decode it 

1. You need to install [inspectrum](https://github.com/miek/inspectrum) for demodulating the signal to decode the file sink 

2. type in terminal ./inspectrum [filename] and you will see the waves

> as you can see in the screenshot, the dude sent a binary message and as you can see the message intercepted are the green vibrated lights are the data (THEIR QUALITY IS SHIT BUT THAT'S THE CHALLENGE)


<p align="center">
  <img src="https://blog.compass-security.com/wp-content/uploads/2016/08/decoding_onoff_keying_inspectrum_load.png"/>
</p>


# Conclusion / Advices

> It's simple, you need to buy devices like RTL-SDR, i would also recommend it for me to buy a new one XD

> While recording, turn off anything electrical or a microwave or anything else

> to conclude everything, firstly you need to send data using a transmitter on a specific frequency except for `radio waves dropping` (that's a different case), secondly you need to detect the frequency (even tho we know the frequency but i wrote it because imagine we are in a real world pentesting you won't know the frequency) using GQRX, after that we convert the data captured to a wav file using GNU-Radio and finally the decoding step using audacity or inspectrum to extract the data.

> I didn't put the setup of arduino uno and stuff because you can find them in google XD

> You can use GQRX to hijack a FM Radio station like changing the frequencies and stuff.

> For defending yourselves, if you are using radio send/receive you need to encrypt your data and be carefull of fake cell towers or rogues based access points (we can also use them to do this shit).

> You shouldn't skip your science classes, you see they helped you intercept your first radio wave XD

**PEACE XD !** 
<p align="center">
  <img src="https://i.kym-cdn.com/entries/icons/mobile/000/011/154/51615.jpg"/>
</p>