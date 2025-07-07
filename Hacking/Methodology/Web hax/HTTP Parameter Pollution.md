What happens if we send the same HTTP parameter two times?
```js
details/?user=john&user=bob
```

Which response will the request return?
Is it John or Bob?

It depends on the backend server.
Some servers return first, some last and others all concatenated.

For example:
- `Java & Tomcat` - first value
- `PHP & Apache` - last value
- `ASP.net & ISS` - All concatenated

This type of vulnerability on it's own might have little impact, sometimes critical impact.
But it can be used in `tandem` with other vulnerabilities to assist in bypassing web app firewall for example.

If we suspect there is a parameter that is vulnerable to `SQLi` but the `WAF` is preventing us from exploiting it.
We can try to abuse parameter pollution and `bypass` the WAF, if it's backend parses the parameters `differently` than the backend of the server.
For example, WAF parses the first parameter, and the server parses the last, then we can bypass the WAF by polluting the parameters.

If the parameters are concatenated, we could spilt our payload into segments.
```sql
id='or&id=%201=1&id=--
--decodes into--
id='or id=1=1--
```

This is called parser differential problem.