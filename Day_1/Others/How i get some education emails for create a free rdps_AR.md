# first_step_bdarija
# بداية هكر   
السلام و عليكم،
ف 2019 همي الوحيد هو ندخل لفلوس من الويب و بديت كندخل لغروبات و نشتارك فقنوات اي حاجة كنت كنجربها واحد النهار بديت كندور فالفايسبوك تا طلع ليا واحد لغروب كيبيعو فيه rdp فهنا فين تزادت عندي فكرة جديدة لي هيا تانا نبيع rdp ولكن فين انلقاه ؟
مشيت قلبت على شنو هو هاد rdp و قلبت كيفاش نقادو فابور لقيت واحد الطريقة من microsoft azure ولكن كتحتاج لإيميل تعليمي .
سولت واحد صاحبي فين نلقى ايميل تعليمي قاليا "هاكي ميسي" (هاد ميسي هو افظل بلاتفورم عند الطلبة المغاربة تما فين كيلقاو النقط ديالهم وخدام طيارة) مهم تانا بديك السنطيحة مشيت بديت نهاكي فميسي اول حاجة خليت dirsearch يقلب لقى ليا هاد الينك
(www.messi.gov.ma/AR/LISTS/SURVEY2/summary.ASPX) 
المحتوى ديالو باج عادية كتحط فيه التقييم ديالك لخدمات ميسي ولكن مع كتحط التقييم كيتحط تا لإيمايل التعليمي ديالك 
بحال هكا 
<center><img src="https://i.ibb.co/tMkjSzx/Screenshot-from-2022-07-24-01-16-09.jpg"></center>
فهاد الباج كاين كثر من الف ايمايل تعليمي دب خاصني نبدا نكوبي كولي كوبيت شي عشرة و نانعيا قلت اجي نشوف واش كاين شي طريقة باش نجيب هاد لايمايلات بلا هاد ما نبقى نكوبي و نكولي فقلبت فغوغل على كيفاش نجيب شي محتوى معين من شي سيت و لقيت بلي كاين واحد الدومين لي كيتسمى الويب سكرابنج 😂 (web scraping) فمشيت قلبت عليه و بديت نتعلم ليه مع ديجا كان عندي شوية مع python جاتني ساهلة خاصني نتعلم غا لواحد المكتبة سميتها
 beautiful soup فبعد متعلمت ليها قدرت نقاد هاد السكريبت  
 
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

### خدمت السكريبت بهاد الكومند 

    ./mailto.py www.messi.gov.ma/AR/LISTS/SURVEY2/summary.ASPX`

### لي طلع ليا هاد النتيجة 
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
```
فدب قدرنا نجيبو لامايلات كيفاش نقاد بيهم rdp ؟ 
دب خاصني نجيب الكود باش نوصل ل email verification 
فمشيت قلبت شوية على smtp sender جبت واحد الكونط من telegram channels داكشي دلكراك و قدرت نقاد واحد letter بالعربية درت فيه "الا ماكدتيش الحساب ديالك غيتحيد فحاول تاكدو بروجولة و زدت شوية د html fields فيهم بلي خاصو يدخل شي معلومات + `الكود`" 
وسيفطها لغاع دوك لايمايلات لي جبت الغد ليه فقت لقيت لايمايل ديالي وصل ليه شي 80 كونط و منها دخلنا صريف .

## <center>خويا طاليب</center>
<center><img src="https://i0.wp.com/hespress.news/wp-content/uploads/2020/07/%D9%85%D8%B5%D8%B7%D9%81%D9%89-%D8%AA%D8%A7%D9%87-%D8%AA%D8%A7%D9%877.jpg"></center>

## <center>انا</center>
<center><img src="https://media.giphy.com/media/LdOyjZ7io5Msw/giphy.gif"></center>
