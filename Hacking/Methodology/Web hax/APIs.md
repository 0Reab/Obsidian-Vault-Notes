APIs in web apps works similarly to http request in a way that it returns data.
For example using GET request we can retrieve json raw data.

For example using curl and proxying request data to [[Burpsuite]].
```bash
curl -X PUT --proxy http://localhost:8080 https://catfact.ninja/breeds -k -d '{name:"cheese cat"}'
# PUT request
# others
# GET, POST, DELETE
```
This curl command attempts to use http put request to add json object.
(Might need foxyproxy browser addon for routing the request thru burp)

```bash
┌──(kali㉿kali)-[~]
└─$ curl -X POST -H "Content-Type: application/json" -d '{"username": "jeremy", "password": "cheesecake"}' http://localhost/labs/api/login.php

Output:
{"status":"success","token":"eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0=.eyJ1c2VyIjoiamVyZW15Iiwicm9sZSI6InN0YWZmIn0=."}
```
token made out of 3 parts comma separated.
header - body - signature is the form

Each section is base64 encoded.
In case where api is returning a token with missing signature aka last comma separated value.
This indicates we can tamper with that data possibly using jwt.io - website.

JWT - json web tokens