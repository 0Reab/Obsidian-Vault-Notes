Cross site scripting vulnerability is a method of exploiting javascript and html 
to run code in html tags or similar.

The basic example is with alert(1) that opens the alert box in your browser, like ones that ask you a permission to allow sharing location with the website or saving passwords.

The JS can be run on client side if the app is vulnerable to XSS, you may think eh can't get a shell that way so no big deal?
Welp hol' up, using XSS we can possibly steal cookies from the users that are using that web app.
There is a condition for stealing cookies via this method, the http only flag is not set (you can check that in developer tools in browser, by looking at the cookie you have).

And also there is a secure flag, which if it's not set means you can transfer the cookie over http or better said on a network that has no tls or ssl.

Testing for blind XSS requires setting up basic python http server to see if anything lands.
And modifying your request with JS to test for XSS and don't forget to encode those parts with URL encode (tip: url encoding has lots of % signs).

If you get something in your python http server it means the XSS is working, and the process of stealing the cookie is a bit convoluted it requires a php server with some js stuff but there is automated scripts to help with that.

This can be found in the wild and it's in OWASP top 10 for a reason, so explore more.

https://www.youtube.com/watch?v=KljtU2vJKPA

**What is the DOM?**  

DOM stands for **D**ocument **O**bject **M**odel and is a programming interface for HTML and XML documents. It represents the page so that programs can change the document structure, style and content. A web page is a document, and this document can be either displayed in the browser window or as the HTML source.

**Exploiting the DOM  
**

DOM Based XSS is where the JavaScript execution happens directly in the browser without any new pages being loaded or data submitted to backend code. Execution occurs when the website JavaScript code acts on input or user interaction.

**Blind XSS**

  
Blind XSS is similar to a stored XSS (which we covered in task 4) in that your payload gets stored on the website for another user to view, but in this instance, you can't see the payload working or be able to test it against yourself first.  
  
**Example Scenario:**  
  
A website has a contact form where you can message a member of staff. The message content doesn't get checked for any malicious code, which allows the attacker to enter anything they wish. These messages then get turned into support tickets which staff view on a private web portal.