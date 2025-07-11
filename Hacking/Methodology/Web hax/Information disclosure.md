Referrers to web application that displays information about it's `web stack` unintentionally.

This can be problematic because adversaries can slowly profile the web applications:
- frameworks
- versions of software
- OS
- database information
- Internal network or directory paths
- and etc...
- configuration files / source code.

All of these things most of the time don't have significant impact.
It really depends on how much information is disclosed and if it can be acted upon.

For example worst case scenario is, the web app uses something that is very outdated and it has a CVE that is critical (something like RCE or similar).
That would be trivial to exploit and could be at least not super obvious by avoiding disclosing version numbers via some error page or else.

Most of the information disclosure happens by error pages, so we can cause errors intentionally.
Or by publicly accessible configuration files or other fingerprinting methods, this one is a bit more rare, hopefully.


But consider, if a web app has one informational disclosure by accident, the odds are there could be more if we keep digging.
The idea being, if the security hygiene is perceived lacking, we could discover more in other places of the application.


**LAB** - information disclosure on debug page. Find `SECRET_KEY` env variable.
**Solution**
Digging a bit, or AKA walking the application, upon viewing the source of one page we see a comment for the debug page.

```js
<!-- <a href=/cgi-bin/phpinfo.php>Debug</a> -->
```

Heading there there's a lot of information and this file shouldn't be accessible.
And the key is found there as well, lab solved.


**LAB** - information disclosure on error page. Website uses vulnerable framework, find it's version number.
**Solution** - `/product?productId=1` - if we set Id to be some random value the error discloses. `Apache Struts 2 2.3.31`


**LAB** - Source code disclosure via backup files. Find DB password (it's hardcoded).
**Solution** - `/backup` is a that is "hidden" and contains source code.


**LAB** - Admin panel has authentication bypass vuln, but It can't be exploited without a custom http header, find the header and delete user Carlos.
**Solution** - `X-Custom-IP-Authorization: 127.0.0.1` This header is a custom http header that this app uses to validate requests origin.
You can find it by using the `TRACE` http method on the blocked endpoint `/admin`.
It responds with all the headers including custom IP one.
But of course the IP will be your public one because it's a trace request, so we simply make it into a local host IP.

The `TRACE` method performs a loopback message test.


**LAB** - Info disclosure in version control history. Find admin password and delete user Carlos.
**Solution** - xd
