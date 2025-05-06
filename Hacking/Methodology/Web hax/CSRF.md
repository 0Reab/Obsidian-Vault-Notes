Means - cross site request forgery.

This vulnerability allows the attacker to perform actions that the users did not intend to perform.
As the name suggests cross-site meaning interaction between two sites is possible when attacker circumvents same origin policy.

That is a flag you sometimes see when inspecting http requests in network traffic for web apps.
For example something like this `Referrer Policy - strict-origin-when-cross-origin` and all else.

The example of an attack could be tampering with the url and requesting actions with parameters that are vulnerable.
Consequences of this vulnerability can be for example, account takeover via email change/ password change and in worst scenario when admin account is compromised to take full control of the said web app.


#### This attack is only possible if these conditions are met:

- A relevant action - a goal in mind that this vuln can achieve - like the examples before - psw change etc..
- Cookie session handling - If there are no other mechanisms in place that identify the user who sent the request - if only the cookie is an identifier.
- No unpredictable request parameters - In short - if you need a piece of information to perform an attack that is not available to you, then it's not really vulnerable (at least for impact).


So an example of a function that meets the conditions of being vulnerable to CSRF.
To make that action user makes a http request like this...

```http
POST /email/change HTTP/1.1 Host: vulnerable-website.com 
Content-Type: application/x-www-form-urlencoded
Content-Length: 30
Cookie: session=yvthwsztyeQkAPzeQ5gHgTvlyxHfsAfE

email=wiener@normal-user.com
```

We got:
- A relevant action - email that is connected to an account.
- Cookie session handling - the cookie is the only identifier of the request sender.
- No unpredictable request params - only the email is a request parameter - in this case we choose a new email to attach to the account (i think).


When we determine this, we can construct a web page with the following contents:
```html
<html>
<body>
<form action="https://vulnerable-website.com/email/change" method="POST">
<input type="hidden" name="email" value="pwned@evil-user.net" />
</form>
<script> document.forms[0].submit(); </script> 
</body>
</html>
```

If a victim visits the attackers web page this is what happens:
- The attackers page will trigger an http request to the vulnerable site.
- If the user is already logged in to the vulnerable site the browser will automatically fill in the cookie in the request (if SameSite cookies are used).
- The vulnerable site will process the request as usual as if the proxy attacker page doesn't even exist and change the email of the account.

Now if the user can authenticate without action along side the cookie such as basic HTTP auth and cert based auth.
The site is still vulnerable because that data is passed alongside cookie without user interaction.

#### Practical lab

The lab had a app vulnerable to the CSRF of course and the solution was to utilize discussed forged html which contains url for email change to the vulnerable web app with correct http method and our attacker email as a payload, which is all send after we deploy this html on our server and user visits the web page, then the email is updated to our email.

Note.
If the app is vulnerable to csrf and it's possible to change email via GET request you don't need to setup an malicious  proxy server with html, the exploit will work with just a tampered url, exactly the same how a reflected XSS can be stored in url.
`<img src="https://vulnerable-website.com/email/change?email=pwned@evil-user.net">` 

####  Defense against CSRF

- CSRF Tokens - 
- SameSite cookies -
- Referer-based validation -