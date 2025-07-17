Server side request forgery

Is a vulnerability that allows an attacker to cause the web server to make additional or edited http request to attackers choosing.

There are two types of SSRF's one where attacker sees what is happening on their screen and other where the attacker doesn't see what is happening, as usual those are called "blind".

The impact of SSRF is access to data/files/areas, scale to internal networks or reveal auth tokens/creds.

This symbols are used to make the url ignore the rest of the url when you inject your url from which you want to retrieve data. 
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

#### Impact of SSRF

- leaking authentication keys
- leaking sensitive data
- maybe RCE
- Allow horizontal pivoting
- Leapfrog server for other attacks

SSRF attacks often exploit trust relationships of the application to gain access to unauthorized actions.
Additionally these trust relationships can bleed into other back-end systems.

#### SSRF attack against the server

For example we have a web app that uses REST API for backend calls to return item inventory for shopping.
Now, admin panel is protected by some authentication system for requests out side of the local network, for remote login.
But for local, there is not authentication.

We can use this knowledge to try and access the admin panel from a local machine by exploiting the REST API by forging a request to go to.
`127.0.0.1` or `localhost` the local loopback addresses and bypass the authentication.

```http
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118 

stockApi=http://localhost/admin
```

How does this security issue happen?

1. `Improper access control` - A check for access might exist in the front end but is lacking when the connection is made back to the server.
2. `Recovery purposes` - In worst case scenario, local machine will always have access to the admin panel, to recover the system in case of creds loss. (*don't do this*)
3. `The interface bind` - The admin interface might not be directly reachable by users or it's on another port than the main application.

These kinds of scenarios when local requests are treated differently than ordinary request, can lead to a `critical` vulnerability.

**LAB** - delete user Carlos.
As it was explained in examples.
`stockApi=http%3A%2F%2Flocalhost%2Fadmin%2Fdelete?username=carlos`
We request the localhost/admin and upon response find in the source URL for button of user deletion and request that URL.

#### SSRF attack against other backend systems

In a scenario where the app is vulnerable to the SSRF and has another backend system within that local network.
`https://192.168.0.68/admin`

These are non routable private IP addresses.
Not accessible by public users.

These systems are only protected by their network topology, meaning being an internal network does not protect them.

**LAB** - Similar to the previous lab, but in this one the goal was to enumerate the API calls internal IP addresses to eventually find admin panel and do the same to delete the user.

#### Bypassing common SSRF defenses

*Blacklist input filters*

1. Use alternative IP address representation `2130706433`, `017700000001`, or `127.1`.
2. Register your own domain that resolves to `127.0.0.1`. You can use `spoofed.burpcollaborator.net` for this.
3. Use URL encoding or character case variation on keywords.
4. Use URL that you control, then redirect to the target URL.
	- Use different redirect http codes.
	- Try switching to and from `http`/`https`

**LAB**:  SSRF with blacklist-based input filter
This lab has a stock check feature which fetches data from an internal system.
To solve the lab, change the stock check URL to access the admin interface at `http://localhost/admin` and delete the user `carlos`.
The developer has deployed two weak anti-SSRF defenses that you will need to bypass.

**Solution** - `stockApi=http%3A%2F%2FlOcAlHoSt%2FaDmIn%2Fdelete?username=carlos`
Alternating case bypasses the blacklist.

#### SSRF with whitelist-based input filters

Because whitelists work on permit only values we can attempt to modify the URL partially.
The filter may look only at the beginning of the URL or some other part.

The URL specification contains numerous features that can be overlooked by this defense.
1. Embed creds before the host name using `@` character - `https://expected-host:fakepassword@evil-host`
2. URL fragment `#` - `https://evil-host#expected-host`
3. Leverage DNS naming hierarchy, `place required input` into `fully qualified DNS name` that you control. `https://expected-host.evil-host`
4. URL encode or even double URL encode, exploits discrepancies in URL parsing.
5. Any mentioned technique combination.

#### Bypassing SSRF filters via open redirection

Using open redirect to bypass the filters by exploiting open redirect vulnerability.
Provided that the backend API supports redirection.
You can construct a URL that satisfies the filter defense and results in redirect request to desired URL.

`/product/nextProduct?currentProductId=6&path=http://evil-user.net`
returns a redirection to:
`http://evil-user.net`

This works by checking if the API request is to the same origin domain as the application which it is.
And the open redirect vuln will trigger upon the API request, and redirecting to the `internal URL` of attackers choosing.


**LAB: SSRF with filter bypass via open redirection vulnerability**
This lab has a stock check feature which fetches data from an internal system.
To solve the lab, change the stock check URL to access the admin interface at `http://192.168.0.12:8080/admin` and delete the user `carlos`.
The stock checker has been restricted to only access the local application, so you will need to find an open redirect affecting the application first.

**Solution** - 
Original API call `stockApi=/product/stock/check?productId=1&storeId=1`

Now we find the open redirect vulnerability, it was a request in proxy after walking the application for a bit. (path parameters is the redirect)
URL: `/product/nextProduct?currentProductId=1&path=/product?productId=2`

So we replace the API URL to open redirect URL - `stockAPI=/product/nextProduct?currentProductId=1&path=http://192.168.0.12:8080/admin`
And again from source to delete the user append this `/delete&username=carlos`

`stockAPI=/product/nextProduct?currentProductId=1&path=http://192.168.0.12:8080/admin/delete&username=carlos`

The reason this works is, the API call appends `/product...` to the http call it will do every time, so we can't really forge a request that is outside of that domain it's set to.
Also remember to URL encode the `POST` data, the API call URL path, otherwise it will not work.

#### Blind SSRF vulnerabilities

This is a bit harder to exploit because the response is not reflected in the front end, thus making it `blind`.
But it can sometimes lead to full `remote code execution` on the server or other backend components.

Considering those facts, more often than not the `impact` of this blind attack is `lower`.
They cannot be trivially exploited to retrieve sensitive data because of their `one-way nature`.

#### How to find and exploit blind SSRF vulnerabilities

The most reliable way to detect SSRF vulnerabilities is using `out-of-band (OAST)` techniques.
**(Out-of-band application security testing)**

In plain English, this means using methods that will allow us to have an interaction with the SSRF vuln and have a response.
That response can be received if we have our `own server` that we can see `DNS queries` for and the `HTTP traffic` it receives/sends.

Just like for `blind RCE` you can use `webhooks.site` to observe `ping/curl/wget` requests and traffic from the victims server.

One easy method is using `burp collaborator` but ain't no body got that money.
It can generate unique domain names, which is what you need and to see their traffic.

*Note:*
When testing for SSRF we often use DNS lookups to our collaborator domain.
If we observe the `lookup` but `no subsequent HTTP` request...
This means that there is `network level traffic filtering` or something of that nature.

It is common to have allowed outbound DNS traffic because so many things rely on it.
But blocking http request to unexpected locations is common practice (I think/hope).

The blind SSRF can be used to probe for other vulnerabilities and misconfigurations of backends systems, enumerating the internal network or maybe achieve RCE.

**LAB** - welp sucks to suck need the burp collaborator to do the lab, you could use your own servers but this won't work for a lab setup like this.
**Solution** - Watch the explainer video at least lul https://www.youtube.com/watch?v=GAQFQhdrM1M

#### Finding hidden attack surface for SSRF vulnerabilities

Other examples of SSRF are harder to detect if the full URL is not in use.

*Partial URL in requests*

If the application places things such as domain names or partial URLs in some requests.
Depending on the context it can be hard/easy to exploit these vulnerabilities.

In other cases where it's hard, the reason being is `we don't control the full URL` thus making our attack surface smaller. (in that context)
Or simply it's exploitability is `limited`.

#### URLs within data formats

As one method of moving data in the web is by using `XML`.
We could chain `XXE` vulnerability with `SSRF` the same way.

The `XXE` - XML external entity injection.
Is a vulnerability type on it's own, and hosts it's own challenges and impact.

But just know it can work in tandem with SSRF vulnerabilities.

#### SSRF via the Referrer header

Some applications use tracking software.

What concerns the SSRF is the tracking is done by `Referrer` http header.
It tells us where did the request originate from (redirected from).

And in some cases that tracking software for one reason or another visits that `URL` of request referrer.
And the same principles of SSRF that we mentioned apply here.