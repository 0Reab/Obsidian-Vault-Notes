#### High level overview

Web applications cache static and public files such as `css`, `js/php` scripts, `images` on cache servers.
So the main application server will not have to serve those files every time.

So this vulnerability can arise when we attempt to deceive the caching mechanism, to store a webpage contents when it's not meant to do so.
If it does store the contents, since the data is cached on a cache server and meant to be served as public non sensitive data, we can just retrieve it.

For example, given a profile URL path, we can attempt to add `/notreal.css` and see if we still fetch the profile contents properly.
If yes, we can test if the server will cache that content by trying to access that same URL with the `.css` file too, and see if the content is returned to non authenticated request.

*Note:* In http requests for cached files there is a header indicating it's age.
`age: 325`

#### Cache keys

When serving content the server can decide if the cached data can be sent or the actual server needs to respond.
This is done using generate cache key, which is made of elements of the http request (URL, &|| body/headers).

If the incoming cache key matches that of a previous request you will get a cached response.

*Note:* If you'd like to explore web cache poisoning topic. See `Web cache poisoning Academy topic`
You will learn to manipulate cache keys to inject malicious content into a web cache.

#### Cache rules

The rules are setup to cache specific files or directories:
1. Static files such as `.css`, `.js`.
2. Static directory rules `/static`, `/assets` - matches URL paths that start with directory names with static files.
3. Filename rules `robots.txt`, `favicon.txt` that change rarely and are required for web operations.

Caches may also have custom rules for URL parameters or dynamic analysis.

#### Detecting endpoints

Identify an endpoint that contains sensitive data and is dynamically fetched and protected by users session.
Use fetching http methods as others are not generally cached.
So use - `GET, HEAD, OPTIONS`

Identify a discrepancy of how origin server and cache server parse the URL path.
- Map URLs to resources.
- Process delimiter characters.
- Normalize paths.

Craft a malicious URL, send it to the victim, and fetch the cached response.


*Note:* 
Make sure every request has a different cache key.
Otherwise you might be served cached responses which can invalidate your testing.

As both URL path and query parameters are included in a cache key, using different parameters will help you differentiate cached responses and help you test.
If you are using burp there is `Param miner extension` you can use it to automatize parameters via `Add dynamic cachebuster`.

#### Detecting cached responses

Various response headers indicate that it's cached:

*X-Cache:* 
Provides information about whether the response was served from the cache.
Typical values include:

1. `hit` - Response was served from the cache.
2. `miss` - Response was served from the origin. (Often, the requests key was not matched, this origin was fetched. Send the request again it should be "hit").
3. `dynamic` - Response is dynamically generated from origin, generally not suitable for caching.
4. `refresh` - The cached content is outdated and needs to be refreshed or validated.

*Cache-Control:*
Header may include a instruction that indicates caching, like `public` with `max-age` > zero.
This only suggest that the resource is cacheable but isn't always indicative of caching.
As the cache may override this header.

If you notice a big difference in response time, this could indicate a different server sending a response.
- Cache server - closer - faster
- Origin server - further - slower


#### Exploiting static extension cache rules

Cache rules often include static files common extensions such as `.css || .js`
This is the default behavior in most `CDNs`.
 (content delivery network)

If there are discrepancies of how the cache and origin server map the URL path to resources or use delimiters.
An attacker may be able to craft a request for a dynamic resource with a static extension that is ignored by the origin server but  viewed by the cache.


#### Path mapping discrepancies

`URL path mapping` is the process of associating files on the filesystem with URL paths.

There are many mapping styles used by different frameworks and tech.
But the most common styles are:
1. Traditional URL mapping.
2. RESTful URL mapping.

**Traditional URL mapping** - represents as direct mapping of URL path to filesystem paths to a resource.
`http://example.com/path/in/filesystem/resource.html`

**RESTful URL mapping** - don't directly match filesystem structure, they abstract file paths into `logical parts of the API`.
`http://example.com/path/resource/param1/param2`

So given this URL: `http://example.com/user/123/profile/wcd.css`

If the path is mapped as a traditional URL the wcd.css has a possibility to be cached if the CSS files are cached.
Otherwise if the RESTful paths are mapped the server will probably ignore the wcd.css file and just fetch the profile path as it does normally.

**LAB** - leak Carlos API key.
**Solution** -
Test if `/profile` is cached by adding `/foobar.css`.
The response says `miss` in the headers, indicating it fetched from origin, after repeating it's updated to `hit` meaning it was from cache.
We make an exploit server JS redirect the user to malicious URL - 
```js
<html>
<body>
<script>document.location="http://lab/profile/foobar.css"</script
</body>
</html>
```
Resend the request to the cached endpoint, same URL the victim visited and you should receive a cached response.


#### Delimiter discrepancies

Delimiters specify boundaries in URL elements.
`?` - is a search query generally.

As the standard is permissive, they vary from framework to tech used.
So discrepancies in how `origin` server and `cache` server use characters as delimiters could cause cache deception.

Consider `/profile;foo.css`

- The `Java Spring` framework uses `;` to add paras known as matrix variables.
	- Ergo an `origin server utilizing` this framework `would` interpret `;` as a delimiter and truncate the path after`/profile` and return profile info.

- Most other frameworks don't use `;` as a delimiter.
	- Ergo a `cache server not utilizing this framework` would `not` interpret `;` as a delimiter but as a part of the path and include `.css` file and cache it.


For another example of delimiter discrepancies.
Consider a Ruby on Rails framework, which uses `.` to specify the response format.

- `/profile` - uses the default HTML formatter, returns user profile info.
- `/profile.css` - recognized as CSS ext. there isn't a CSS formatter so request returns error. (req not accepted)
- `/profile.ico` - icon extension isn't recognized by Ruby on Rails, the HTML formatter returns user profile info.
	- But in this instance the cache is configured to store responses for requests ending in `.ico`, it will cache and return profile info as if it were a static file.


Another example with encoded characters as delimiter.
`/profile%00foo.js`

- The `OpenLiteSpeed` uses the encoded null byte as delimiter character.
	- Therefore the origin server using `OpenLiteSpeed` would interpret this as `/profile`
- Most other frameworks respond with error if null byte is in the  URL.
	- However if the cache server uses `Akamai` / `Fastly` it would interpret the null byte and the file in the path.

#### Using delimiters

So to test for delimiters ideally you can identify what server is in place or by recon.
Otherwise enumerating the URL with different delimiters and see what requests go thru with omitted delimiter data.
But be cautious because sometimes the server could be redirecting you, then you will need a different endpoint.


Logically you would want a scenario where the origin server uses a delimiter that the cache server does not.

Make sure to test multitude of file extensions for caching and for delimiters.

*Note:* If the delimiter is either of `{`, `}`, `<`, and `>`, and use `#` truncate the path then you will have the check if the server decodes the URL encoded characters like these.
Because by default sending this to a victim, the browser will URL encode these characters and the cache deception could fail.


**LAB** - path delimiters for cache deception, find API key of user Carlos.
Delimiter list: https://portswigger.net/web-security/web-cache-deception/wcd-lab-delimiter-list

**Solution** - A bit of brute-forcing delimiter, only non error was character was `;`.
```js
<script>document.location="https://0a5a004c049e05bf8107ad7300c60048.web-security-academy.net/my-account;foo.js"</script>
```
Using the same exploit server JS we redirect user to a cached file.
After they visit the URL we can visit the same URL and receive the cached data.
*Note:* Next time pay attention to make each caching unique because I don't think if I reuse the payload it will refresh the cache. And remember always pay attention to hit, miss headers. 

#### Decoding delimiter discrepancies

In a scenario where the delimiter in the URL is URL encoded.
We can run into discrepancies depending if the origin and cache server decode the URL encoded characters.

Consider `#` character.
In a URL `/profile%23wcd.css`

So the difference in how the URL is parsed will decide how the URL is treated.

If the origin server decodes the `%23` into `#` , then the URL is seen as `/profile`.
Otherwise if the parser in the origin server does not URL decode the character it will pass unchanged and can be used for cache deception.

Additionally it matters `when` does decoding of the URL happen.
For example if we receive the URL with encoded delimiter which the origin server uses.

The cache server could apply caching rules using the URL then do the URL decoding.
Then forward the request to the origin server whom will see decoded char as delimiter and do the request.

Also test for non printable characters such as `%00`, `%0A` and `%09`.
If these chars are decoded they can `truncate` the URL path.

#### Exploiting static directory cache rules

It's common practice the servers store static files in specific directories.
`/static, /assets, /scripts, /images, /js`

These rules can also be vulnerable to web cache deception.
*Note:* To exploit these types of rules, you need to be familiar with path traversal vulnerability.

#### Normalization discrepancies

When URL paths are normalized, this involves decoding URL encoded characters and resolving dot-segments in paths.

Following the same principle like previous discrepancy based cache deception attacks.
We can use this to our advantage and exploit cached static directories such as `/assets` with path traversal.
For example:
`/static..%2fprofile` 

This can resolve into `/profile` by the origin server, simply omitting the `/static` part thus returning the profile data for caching because of the directory caching rules.
Because in this discrepancy the caching server sees the whole path and triggers a caching rule on `/static..%2fprofile`

#### Detecting normalization by the `origin` server

To test if the paths are normalized by the origin server we need a non-cached endpoint.
This can be done via POST request or others.

So for example `/aaa/../profile` is our path traversing payload.

If the origin server return the profile page normally we know that the path traversal has been normalized into `/profile`.
Otherwise if get 404 then our traversing failed.

*Note:* It is important to encode only last slash because some CDNs match the slash following the static directory prefix.
Or alternatively a dot instead of a slash, or all path traversing sequence.

#### Detecting normalization by the `cache` server

Same thing for the cache server.
If we find a resource that is likely to be cached considering it's directory and file type.
`/assets/js/stockCheck.js`

After confirming it's caching we can attempt to add path traversal to the URL and observe if the path is cached or not.
If we try `/aaa/..%2fassets/js/stockCheck.js`

And the response is cached that means that the cache server normalized the path back to it's original URL.
If the response is now not cached we can conclude that the path has not been normalized.
We can assume that the `/assets` prefix is a caching rule.

Another test is to add path traversal after the directory prefix.
So `/assets/js/stockCheck.js` again, into `/assets/..%2fjs/stockCheck.js`

By this we can attempt to satisfy a caching rule that caches based on `/assets` prefix.
Therefore if the caching fails we know that the path has been normalized into `/js/stockCheck.js` which does not satisfy the `/assets` caching prefix rule.
Otherwise if it does cache we know the path has not been normalized and it stayed the same.

In conclusion you can't tell for certain of the normalization occurs or not without attempting the exploit.
But you can get the feeling for it's behavior by testing.
Additionally if you suspect you can try caching `/assets/aaa` and determine the prefix rule.
There might be other rules in caching play that we are unaware of, so keep that in mind.


#### Exploiting the normalization by the `origin` server

For example a path `/assets/..%2fprofile`:
- The cache interprets the path as is, no normalization, caches the response.
- The origin normalizes the path and returns profile info.


**LAB - Exploiting origin server normalization for web cache deception**
To solve the lab, find the API key for the user `carlos`. You can log in to your own account using the following credentials: `wiener:peter`.
We have provided a list of possible delimiter characters to help you solve the lab: [Web cache deception lab delimiter list](https://portswigger.net/web-security/web-cache-deception/wcd-lab-delimiter-list).

**Solution** :
```js
<script>document.location="https://0a8900ea033c86928194348c007100d7.web-security-academy.net/resources/..%2Fmy-account?&fi=bi"</script>
```
After browsing the web app for a bit, to allow requests to flow thru the proxy.
We apply scope filter for easy lookup, and I notice multiple JS scripts originating from `/resources` path and all are cached.
Utilizing the path traversal the server for caching is not normalizing the path and hitting `/resources` prefix caching rule.
And the origin server is normalizing the path into `/profile` allowing it's normal response fetch and caching.

#### Exploiting the normalization by the `cache` server

If the cache server is normalizing the path but origin is not.
We can construct this kind of payload.

`/dynamic-path/../static-dir-prefix`
In other words...
`/profile/../assets` - for example

*Note:* When exploiting path traversal in cache server normalization URL encode the whole path traversal sequence!
`/profile%2f%2e%2e%2fstatic`

- The cache server is likely to interpret the path as `/static` as expected.
- The origin server interprets the path as is `/profile%2f%2e%2e%2fstatic`

*Keynote:*
The origin server is likely to return an error, so path traversal is not enough for this exploit thus we will combine the URL delimiter as mentioned before to truncate the path.

This exploitation is the same concept as the previous explainer of normalization of path in the origin server, but reversing who does the normalization.
And we simply just run into origin not fetching normalized URL paths, ergo the delimiter need to allow actual data fetch by origin.
`/profile;%2f%2e%2e%2fstatic`

(under the pretense that the cache server doesn't use that delimiter as origin.)


**LAB: Exploiting cache server normalization for web cache deception**
To solve the lab, find the API key for the user `carlos`. You can log in to your own account using the following credentials: `wiener:peter`.
We have provided a list of possible delimiter characters to help you solve the lab: [Web cache deception lab delimiter list](https://portswigger.net/web-security/web-cache-deception/wcd-lab-delimiter-list).

**Solution** - Found %23 - `#` can be used as a delimiter for the origin server.
Same path is cached as before `/resources` contains JS stuff.
So we combine the `/profile#/../resources?&a=b`
The path traversal is URL encoded whole `/../` into `%23%2F..%2F` and we have a cache identifier a=b which we can change when delivering exploit.
The `#` is used to delimit the path and allow origin server to fetch the dynamic response.
And the cache server does not care about `#` and normalizes that path into `/resources` triggering directory prefix caching rule.

#### Exploiting file name cache rules

Certain files such as `robots.txt`, `index.html`, and `favicon.ico` are common on web servers.
They are often cached because of the infrequent changes.

These caching rules usually go by exact filename.
So try `GET` on some of them and see if it hits.

This is susceptible to the same vulnerability as other path normalization vulnerabilities.
`/profile%2f%2e%2e%2findex.html`
So using path traversal could end up caching the response of the `/profile` if the caching server normalizes the path into `/index.html`
If the response isn't cached, this probably means the path was not normalized and taken as is.

As with other normalization vulns this will only work if the cache server normalizes the path and the origin doesn't, simply because the caching rule is exact file name matching.

#### Preventing web cache deception vulnerabilities

1. Always use `Cache-Control` headers to mark dynamic resources, set as `no-store` and `private`.
2. Configure your CDN to not override the `Cache-Control` header.
3. Use built in CDNs protection against web cache deception.
	- It verifies if the `Content-Type` header value matches the URL file ext.
4. Verify that there are no discrepancies between URL parsing of origin and cache servers.