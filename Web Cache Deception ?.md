# Web Cache Deception ?

Hi fellows ! This gonna be my 1st subject on the *Hacking Week event*, we gonna explain a bit what is Web caching and how attackers takes advantage of this functionality for their profit .


# Caching what is that ?

caching in general is a process of keeping a copy of resources in a data storage area in order to be loaded faster in the next time you need it , for example browsers can store website's data (images,js,html files) this process can improve user experience and also reduces bandwidth usage . this was browser caching but this is not the kind of caching we want to talk about , what we want is servers that can be used for caching .

> ##### Benefits of Caching :
> -  Improve Application Performance.
> -  Reduce the Load on the Backend.
>  - Availability of content during network interruptions.
>  - Sometimes can get you some bucks on bug Bounty (that was a joke , or not) .

## Web Caching servers .

- #### Load Balancer :

when you have a big base of clients that requests your server ,this might slow the server's performance and cause some losses , then  load balancers are can be one of the best choices , lets imagine we have **thnb.com** .

![img1](https://i.imgur.com/Jz9lNNm.png)

1 clients to 10 per day isn't a big deal , but what if  we have like 10000 requests per second ? then we have a problem here . so time to setup a load balancer .

![](https://i.imgur.com/0YjRrFy.png)

as simple as it looks load balancer works as intermediary server setting in front of your internal servers in order to distribute incoming traffic , Web1,Web2,Web3  its a copy of the original website  , so whenever a client visits our website  the intermediary server will route his requests  to one of the internal servers using a specific algorithm . (this was an example that will help us better understand the caching process in the next sections . )
> there is a lot of caching proxy servers , applications out there you can check them and their documentations .
>  - NGINX
>   -  Varnish Cache
>    - Squid Proxy




## Web Caching process 

![](https://dl.acm.org/cms/attachment/d3fb9bb4-bfaf-48fe-8d3a-97bea0364337/proxy-1.jpg)

the image explain the caching process in a simple way .

1 :  Client requests web page for the first time .
2 : the requests going through the proxy server and asks the internal web server for the response .
3 : internal server respond to the requests .
4 : proxy server saves a copy of the response in the cache area and send the response to client 
**(side note :  depending on the configuration some of the data can be cached and other doesn't for example we can say to the proxy server if the file are larger than 1MB or if it is a js file cache it  ...)** .

so the next time if the client or any other client asks for the same request the proxy server respond from the cache and will not bother the internal server by asking again for the response .

## Let's get down to business

![](https://memegenerator.net/img/instances/82086387.jpg)

so let's assume you are logged and  thnb.com/home.php reveals some of your informations (email,username,API token etc ..) you are the only one who can see them  , sure you have session ID and you provided your valid credentials .
and the proxy server configured to cache any static files css,js,images ... ,  as a hacker / pentester after your recon you found out that if you sent a request  like thnb.com/file.php/testme.css it returns back 200 OK  and the content of file.php, ????

![](https://i.pinimg.com/736x/ce/82/c9/ce82c96b20ccda0c60c4f8060e81f228--strict-parents-where-are-you.jpg) 


as said before  our proxy configured to cache any static file , our response came back with 200 OK , but before  that the response passed through the  proxy server and he thinks that the url  thnb.com/file.php/testme.css is a static file and from here you know the deal , he cached it yes :D . 

think a bit ... , what if we sent  thnb.com/home.php/givemeyoursecretes.css to a logged-in user ? home.php contains sensitive infos  the proxy server will cache it and the attacker can access it 


![](https://www.riskbasedsecurity.com/wp-content/uploads/2016/02/escalate.jpg)

web cache deception  reports:

[shopify web cache deception](https://hackerone.com/reports/1271944#:~:text=Web%20cache%20deception%20%28WCD%29%20is,access%20to%20that%20cached%20data.)

[Valve Web cache deception](http://blog.takemyhand.xyz/2018/05/web-cache-deception-in-one-of-valves.html)
