# How I managed to hack a panel that I could have used to earn a lot of money

# Note: this is not BlackHat don't judge from the title.

## Hacking-week-day-2
Hey, what's up guys! My life is filled with stories and this story is special as I'm about to tell you about an event that Reskin-related. In short, for those who don't know what application reskinning is, basically it's modifying for example you take an application and you change its package name, the logo, name of the app, version, colors, etc... and you either look for a source code from a known source or you buy it from ```codecanyon``` for example.

So, during the pandemic and quarantine time, I didn't have much to do so I thought about reskinning apps and publish them in playstore and at that time, wallpaper applications were trending and there were 2 source codes that were known as most people used to buy them. I got my hands on them and these apps are easy, inside the source code there was a "php panel" that you need to host in a server that you buy from namecheap or bluehost for example. Most people used to buy from namecheap because it is cheap, obviously, so I took that php panel and put it in the localhost, I ```analysed``` it and found the ads of ```Admob``` that you put in the panel.

## ADS
- App id
- open app Ad
- Banner ad
- Interstitial ad
- Natif ad

![Logo](https://i.imgur.com/IWUZnTh.png)


And those wallpapers that are shown to the users who have the app are from the php panel, even if the creator of the app wanted to delete a wallpaper or add another one, he does it from the panel. Unlike the older source codes as wallpapers used to be integrated inside the source code and you couldn't add or delete anything until you do an update.
As you can see, there are a lot of platforms for mediation such as :
- Facebook
- StartApp
- Unity Ads
- AppLovin's MAX
- Mopub

![Logo](https://i.imgur.com/RhG9Ivd.png)

These are the ads that appear in the application and whenever users click on them and that app was downloaded a lot, it means that a lot of users are going to click on the ads and you earn money from those clicks. So I put the other source code 'that I got my hands on for free' in the localhost and ```analysed``` it briefly, just to find that it was protected with a ```licence_key``` and I found it easy to bypass that ```licence_key```
As you can see in the picture

![Logo](https://i.imgur.com/qs90z6A.png)


I went and omitted the function the checks the ```licence_key``` and I changed ```else``` and "the location that was inside the ```header``` so even if the ```licence_key``` was ```wrong```, it would take me to the ```dashboard```. And that's exactly what happened, after that, I went to the dashboard file and I also omitted from it the function that checks whether the ```licence_key``` is in the database

![Logo](https://i.imgur.com/PsXC00l.png)

I bought a host from namecheap and I uploaded the php panel, after that I opened the source code with AndroidStudio. I changed ```ADMIN_PANEL_URL``` in the ```config``` with the ```link of the php panel ```that was uploaded in my host.

![Logo](https://i.imgur.com/JLbcRfI.png)

I noticed the ideas and wallpapers that were trending and the apps that were installed a lot, so I looked for another niche to work on. Everytime I'd download an application from PlayStore, I would find the same source code that I have. I took the ```package name``` of an app from a friend of mine and looked for it in ```apkpure``` and downloaded the app in the form of ```.apk```. I reverse-engineered the app using ```apktool```, after it gave me the resutls, I opened the folder ```output``` using ```Atom IDE``` and I searched the output folders for ```http:// and https://``` trying to look for a host in that app. I found it and tried to log in using ```admin admin``` and it worked! You can go to ```manage ads``` and change his ads and put yours and you can earn a lot of money, especially that this app was installed by +1M users.

A lot of people forget their default credentials and here one can enter and erase his wallpapers even if he changed the ```user and password```, it's already too late because I found another way to bypass the ```authentication```.

So there is a lot of danger on apps in PlayStore that have the same source code like those mentioned previously that were trending.

I started attempting to access the dashboard and I found a way to bypass the login even if the ```username and password``` were ```incorrect```. I ```fixed this problem in my php panel``` in case someone found this way and wanted to test it on me hahaha.

As you can see in the pictures..

![Logo](https://i.imgur.com/STaQugY.png)


![Logo](https://i.imgur.com/rellBnU.png)

I found another way to bypass "extensions allowed" because the ones that were allowed were :
- jpg
- jpeg
- gif
- png
- mp4

![Logo](https://i.imgur.com/C64lHQa.png)


Which means I can upload any format of a file, I tried to upload a reverse-shell as you can see in the picture.

![Logo](https://i.imgur.com/1dqvD9L.png)

![Logo](https://i.imgur.com/O1IQmuI.png)


Here I uploaded the shell and established a back-connection on my host.

![Logo](https://i.imgur.com/2A9I3Bv.png)


I talked to the developer and told him that this is ```dangerous``` because you can change the ```ads``` of other people with yours and someome people have 40+ apps with the same source code and I could have done this to his demo panel in ```codecanyon``` to prove this to him but it is illegal. After he saw the message of me explaining to him, instead of talking politely and in good manners; he started saying he wouldn't forgive me and that I study in advanced schools that taught me these skills and I apply them on him. I merely gave him an advice in case this method fell into the wrong hands. I don't to publish my messages between me and him due to personal matters.

I found that they were a big team that made this source code and they couldn't fix this problems and publish a free update to this code for people who bought in the first place because there more than ```+1000 apps``` that use this source code. The ```owner``` of the ```second source code``` didn't reply and set his website to be ```under construction.```

Which means all people who use this source code are in danger and I didn't harm anyone but the problem is if someone finds this method, he wouldn't be as merciful and he would change other people's ads with his and would earn lots of money

I hope you liked this story! bye!

## ```By Theinjector```
