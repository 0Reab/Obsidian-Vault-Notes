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

