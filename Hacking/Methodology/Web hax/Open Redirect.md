Is simply a redirect from one website to another.

You may have seen a redirect like this.
```js
abc.com/?url=xyz.com
```

If we alter the URL parameter.
```js
abc.com/?url=attacker.com
```
This would be the basic idea of open redirect.


Why do we need these redirects?
Often times websites redirect you to places such as:
- `login`
- `register`
- `checkout`
- `create
- `passwordreset`

![[Pasted image 20250707145554.png]]

This is an example of server side redirection.
Other times there is client side redirection, via Java Script/Html.

If it's JS, more often it's `window.location` object (it holds current page URL).
Or if it's HTML it's going to reside in `<meta>` tags.

Open redirects can be common, depending on application.
But are they a security threat?
It depends...

They can be used for fishing or `social engineering attacks`, but so can many other things.

Here is a another social engineering  scenario.

Consider a google URL that has an open redirect.
Upon user clicking on it, they get redirected to attacker.com where the automatic download starts and immediately redirects them back to google "thank you for downloading" page.


A non social engineering example.

Password reset page with http parameter `login` that has an open redirect.
In a scenario like this the password reset token is in the URL as a parameter.
It's there because those links usually originate from the email upon requesting password change.

We can utilize the open redirect of the login parameter to redirect to attacker.com
And the request from the victim, which will have a `Referrer` header set to `current URL`  because redirects set this header.
Meaning we get the token for password reset sent to the attacker.

This is all roughly speaking example.
That URL with open redirect would be hard to come by without prior vulnerabilities being exploited like reflecting "next" parameter in the email with custom URL.

And the Referrer header might not be set if the server is configured to have Referrer policy set to none or strict origin.

But in a nutshell this vulnerability can be chained with lots of others web vulns such as SSRF and XSS...