
# My First Step
## How i get some education emails for create a free rdps


>     “I knew I never should have trusted my best friend,” says Sarah Johnson of Lawrence, Kansas.

actually i don't know why i write this but i search how i can start an article then i find *start it with a quote* :joy: 
\
btw in 2019 i spend all my time in ***"how to earn money from the web"*** , after some scrolling in (youtube, facebook etc..) i see people getting hundred of dollars from selling rdps, then i get this new interesting idea, *i ask google* **"How to get free rdp"** i find a methode in microsoft azure with education email, i ask a friend how to get some education emails he tell me ***'hacki messi'*** `messi is an moroccan government platform for students`, with my sntiha  i go to hack messi :joy: for get edu maillist i start with some enumeration i find www.messi.gov.ma/AR/LISTS/SURVEY2/summary.ASPX `is an simple page for students feedback`, but when the student submit his rate the site post the student rate + his edu_email .
<center><img src="https://i.ibb.co/tMkjSzx/Screenshot-from-2022-07-24-01-16-09.jpg"></center>now we have emails but how to get the rdp i need password for email verification ?

hmm... first how i can copy all this emails it's more than 1000 email, it's hard to copy paste.

- [x] web scraping
- [ ] copy past

> "how to get specific content from html page", *let's google it*,
> 
i find  a video about scraping web pages with python then i start to learn about python web scraping i learn about <red>beautiful soup</red> library and i code this simple script:
```python
import requests
import sys
from bs4 import BeautifulSoup
try:
    url = sys.argv[1]
    resp = requests.get(url).text
    soup = BeautifulSoup(resp, 'html.parser')
    mails = soup.select('[href^=mailto]')
    for mail in mails:
        print(edu_mail.text)
except IndexError:
    print("usage: ./mailto.py <url>\n[*] ./mailto.py https://example.com")
```
let's run it with
`./mailto.py www.messi.gov.ma/AR/LISTS/SURVEY2/summary.ASPX`
### The resulte :
```ruby
N146000439@education.ma
K110094394@education.ma
R136545555@education.ma
H143013247@education.ma
R133265971@education.ma
G136329252@education.ma
G132277788@education.ma
G135526251@education.ma
N135314217@education.ma
D132283147@education.ma
H131116402@education.ma
C138094831@education.ma
D139632313@education.ma
R131204923@education.ma
R132389661@education.ma
D138487025@education.ma
J138249256@education.ma
D136210524@education.ma
H130010996@education.ma
H133344139@education.ma
D135889831@education.ma
R130149159@education.ma
K132510145@education.ma
etc...
```
now we have a maillist with more than 1000+ education email how to use it to get rdp ??
\
let's try some phishing skills i created a simple letter with arabic language the letter content means
"Your messi account have been blocked, please check your informations or you gonna lose your account + some html input fields like (full name, email, num, `password`...)"
and i take advantage the end of education season for more interaction from the studens.
\
after 24h i check my inbox i find more than 80 logs let's get money .


## <center>Talaba (students)</center>
<center><img src="https://i0.wp.com/hespress.news/wp-content/uploads/2020/07/%D9%85%D8%B5%D8%B7%D9%81%D9%89-%D8%AA%D8%A7%D9%87-%D8%AA%D8%A7%D9%877.jpg"></center>

## <center>Me</center>
<center><img src="https://media.giphy.com/media/LdOyjZ7io5Msw/giphy.gif"></center>
