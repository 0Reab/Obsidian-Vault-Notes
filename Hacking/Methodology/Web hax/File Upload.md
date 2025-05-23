File upload vulnerabilities can happen when web app functionality to upload a file lacks security checks such as file:
- type
- contents
- size
- name

This can lead to `script execution` on the server by requesting the uploaded file.
Potentially leading to a `shell`.

If the file isn't validate properly it could also lead to `overwriting` critical files on the system.
In tandem with `path traversal` vulnerability it might be possible to overwrite files outside the intended file upload location.


File `overwriting` can be done by simply using the same filename of the file being uploaded as the file on the server.
Validating `file size` prevents `DoS` attack where disk space is filled by file upload.


#### File Upload handling

Given how dangerous this vulnerability can be, it is very rare to find an unrestricted file upload functionality.
But more often it will have some sort of "robust" validation in place.

For example using `blacklist`, which can fall short when some `obscure filetypes` are missing from it.
Or a fail to check for discrepancies in file `extensions`.
In other cases the defense is checking for properties that are easily manipulated in a web request.

And even if robust file validation is in place, it's possible that is not consistently applied across the hosts and directories.


#### Requesting static files

In the past `static` files on the web servers could be mapped `1:1` to the filesystem containing them.
Nowadays websites are more `dynamic` and often path of a request has almost no relation to filesystem.
But regardless there are still static files, such as CSS, images and else.

The process of handling static files are mostly the same.
The requested path is parsed for file extension.
And depending on it's type the server will either send the data in HTTP response for example an image.

Or in case of an executable script...
The `variables` are assigned based on the headers and parameters of the request before running the script.


In other cases where the file is executable script but the server is not configured to run them.
It is possible they are going to be returned in plain text or cause an error.
Such misconfiguration can be exploited to `leak source code` more on this in information disclosure note.

The `content-type` header may provide clues as to what kind of file the server thing it served.
Otherwise it normally contains the result of extension/MIME type mapping.

#### Web shell

Uploading of a web shell and command execution has been covered in `Server-Side vulnerabilities` note.

#### Flawed validation

When submitting HTML forms the data is sent via `POST` request.
With content type `application/x-www-form-url-encoded`.

This is fine for sending short strings such as name, address etc.
But for larger amounts of binary data, `multipart/form-data` is preferred (images, PDFs).


For example consider an image upload form, alongside it's description and your username.
Submitting this form would result in a following request.
```http
POST /images HTTP/1.1
Host: normal-website.com
Content-Length:
12345 Content-Type: multipart/form-data; boundary=---------------------------012345678901234567890123456

---------------------------012345678901234567890123456 
Content-Disposition: form-data; name="image"; filename="example.jpg
Content-Type: image/jpeg 

[...binary content of example.jpg...]

---------------------------012345678901234567890123456 
Content-Disposition: form-data; name="description"

This is an interesting description of my image.

---------------------------012345678901234567890123456
Content-Disposition: form-data; name="username"
wiener
---------------------------012345678901234567890123456--
```

Request body is split into parts.
Each containing `Content-Disposition` which relates to it's respective input field.
Each of those parts can contain `Content-Type` which declares MIME type for the server of that form.

One way web apps might try to validate the upload is by implicitly trusting the `Content-Type` matches an expected `MIME` type.
If no further validation is done, this can be easily bypassed.

Just to reiterate - `Content-Type` is an HTTP header in your POST request, and `MIME` type is server side list of allowed file types.


**LAB** - image upload Content-Type bypass.

Just like in a lesson, Content-Type as image/jpeg bypasses it.
But one weird thing, in Firefox dev tools when I edit the request I end up getting a response that I'm missing my CSRF token, like I omitted part of my request.
While in burp the request goes thru no problem.


#### Prevent file execution in user accessible directories

While preventing bad file uploads in the first place is better, as a second line of defense is `stopping their execution`.
As a precaution servers execute only mime types who's type they are configured to execute.
Otherwise they could return an error or return the script in plain text.

While this `prevents` any web shells, it may allow for source code leakage.

This configuration differs between directories.
Directories with user supplied files will have much `stricter rules` than others.
Ergo if it's possible to upload and reach files outside of those directories, the server might execute the script after all.

**LAB** - Exploit file upload vulnerability and bypass no script execution with path traversal vulnerability.

Solution:
So the file upload had no restrictions.
And initially I had the right idea to modify the request filename parameter to traverse paths.
```http
Content-Disposition: form-data; name="avatar"; filename="../../payload.php"
```
But I did not expect that I would have to bypass path traversal defense, considering upload itself was unrestricted.
Regardless of that, what I can gather from this is actually reading the response next time.

On each file upload server would respond with "file uploaded at /path/file.ext successful"
By that response, I could've inferred that `file.ext` in response comes from my `POST` request.

Meaning that if path traversal was successfully injected in the filename it would look like this.
"file uploaded at /path/../file.ext successful"

But in this lab, the path traversal defense can be bypassed by URL encoding the forward slash which is `%2f`.
```http
Content-Disposition: form-data; name="avatar"; filename="..%2fpayload.php"
```

I guess lesson learned, we are doing hacking and things are going to be tricky even while learning.


#### Insufficient blacklisting

Blacklisting is inherently bad in this context.
It can easily happen that a dangerous filetype is missed.
For example:
- `.php5`
- `.shtml`

#### Overriding server configuration

By default servers such Apache will not execute php files upon request.
Developers would have to setup `/etc/apache2/apache2.conf` file.

```python
LoadModule php_module /usr/lib/apache2/modules/libphp.so
	AddType application/x-httpd-php .xd
```

Many servers allow configuration override/add on directory basis.
For example Apache servers will load directory specific configuration  `.htaccess` if present.


For ISS servers.
`web.config`
In this case it allows JSON files to be served.

```python
<staticContent>
	<mimeMap fileExtension=".json" mimeType="application/json" />
	</staticContent>
```

**LAB** - exploit file upload vulnerability to read contents of a file.

This one was a bit tricky, the main idea is that the server is Apache.
And as mentioned, file execution per directory basis is done via `.htaccess` file.

So, after uploading file named as `.htaccess` and entered `AddType application/x-httpd-php .xd`.
We overridden the rule of file execution for this directory.
Now it can execute `.xd` files as php application scripts.

The file extension was chosen arbitrarily.


#### Obfuscating file extension

Even the most robust blacklists can be bypassed by obfuscation.

For example if blacklist validation fails to recognize that `.pHp` is `.php`.
And backend that maps file extensions to MIME types is `not case sensitive`.

This could lead to code execution.

List of other techniques:
- Multiple extensions - `exploit.php.jpg` could be read as either php/jpg.
- Trailing characters - `exploit.php.` characters could be ignored or stripped, causing blacklist to fail on a case. `.php != .php.`
- URL encoding - `exploit%2Ephp` for dots & slashes - if the filename is decoded after validation it will bypass blacklist, (try double URL encoding too). 
- Semicolons & null byte - `exploit.php;.jpg` & `exploit.php;%00.jpg` if the server validates filenames in low-level functions C/C++ then (; & %00) can cause problems.
- Multibyte Unicode chars - `exploit\xc0\x2ephp` bypasses validation, server decodes and normalizes malformed characters into a dot = `exploit.php` (you smuggle a dot).

