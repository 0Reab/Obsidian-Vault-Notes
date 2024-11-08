Server side request forgery

Is a vulnerability that allows an attacker to cause the web server to make additional or edited http request to attackers choosing.

There are two types of SSRF's one where attacker sees what is happening on their screen and other where the attacker doesn't see what is happening, as usual those are called "blind".

The impact of SSRF is access to data/files/areas, scale to internal networks or reveal auth tokens/creds.

This symbols are uset to make the url ignore the rest of the url when you inject your url from which you want to retrieve data. 
**&x=**

The target URL where the data is we want to retrieve:
https://server.website.thm/flag?id=9 

Now the URL we are attacking:
https://website.thm/item/2?server=api

The attack will be an URL injection with payload server.website...
https://website.thm/item/2?server=server.website.thm/flag?id=9 

and at the end add &x= to make it work.

If you are curious about the &x= https://security.stackexchange.com/questions/263850/what-is-the-effect-of-the-x-in-a-ssrf-is-it-something-related-to-encoding

The idea is that url making a functionality of & and x= this parameter equals nothing in this case.
Which just interrupts the logic processing the url and just does the request on the payload url without running the rest as it combines  the url in the attack.
(this might be completely wrong god help).

actual explanation from link:
According to the RFC 3986 Regular Expression of `^(([^:/?#]+):)?(//([^/?#]*))?([^?#]*)(\?([^#]*))?(#(.*))?` does not allow parsing of another `?` after a `&`, which denotes a second and so forth parameter.

I guess it just turns rest of the url into a parameter string x?

---------------------------------------------------------------

Potential SSRF vulnerabilities can be spotted in web applications in many different ways. Here is an example of four common places to look:

**When a full URL is used in a parameter in the address bar:**

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/956e1914b116cbc9e564e3bb3d9ab50a.png)  

**A hidden field in a form:**

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/237696fc8e405d25d4fc7bbcc67919f0.png)  

**A partial URL such as just the hostname:**

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/f3c387849e91a4f15a7b59ff7324be75.png)

  

**Or perhaps only the path of the URL:**

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/3fd583950617f7a3713a107fcb4cfa49.png)

Some of these examples are easier to exploit than others, and this is where a lot of trial and error will be required to find a working payload.

If working with a blind SSRF where no output is reflected back to you, you'll need to use an external HTTP logging tool to monitor requests such as requestbin.com, your own HTTP server or Burp Suite's Collaborator client.

Some targets can be local host as well.
Because we are executing that url on the server side, and fetching it to us.
