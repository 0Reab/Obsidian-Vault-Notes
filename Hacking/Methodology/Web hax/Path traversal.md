This vulnerability allows the attacker to read the files on the server.
It might also include:
- Application code and data.
- Credentials.
- Sensitive OS files.

In some cases the attacker can write to arbitrary files on the server.

#### Basic example

The web app displays an image via `<img src="/loadImage?filename=218.png">`
`loadimage` URL takes a `filename` parameter to load an image.

Images are stored in a path `/var/www/images`
To return an image, application appends the filename to the path and uses a filesystem API to read file contents.
So resulting in `/var/www/images/218.png`

In our example there is no protection against path traversal attacks.
So, an attacker can request the following URL and read contents of `/etc/passwd` (always readable).


`https://insecure.com/loadimage?filename=../../../etc/passwd`
This ends up being this path.
`/var/www/images/../../../etc/passwd`

Which is a valid path and `../` means to go one directory up.
And effectively you end up on `/` after that is passed in three times, as we are three directories deep.

For windows this path traversal is valid, as well as with backwards slash.
`..\..\..\windows\win.ini`


#### Defense bypass

If an application uses user input for file paths they often implement defenses for path traversal.
Those can often be bypassed.

**Absolute path**
If it strips or blocks user input there are techniques of bypassing it.
For example using an absolute path from the system root to reference a file instead of traversing `/etc/passwd`.

**Nested traversal sequences**
`....//` or `....\/` -> `../`
These revert back to simple traversal sequences when the inner sequence is stripped.

**LAB** - `filename=....//....//....//etc/passwd`

**URL encoding**
Sometimes when the URL path or parameter have defenses, it will strip any path traversal sequences.
In some cases that can be bypassed by `URL encoding` the `../` chars (even two times encoding).
Resulting string is `%2e%2e%2f` and `%252e%252e%252f` respectively.

**LAB** - `%252e%252e%252f%252e%252e%252f%252e%252e%252f/etc/passwd`

**Validation of start path**
If an application is expecting a base directory such as `/var/www/images` in this case it might be possible to bypass by combining base directory with path traversal sequence.

**LAB** - `filename=/var/www/images/../../../etc/passwd`

**Null byte - file extension bypass**
If an application is expecting a specific file extension for a file such as `.png` it might be possible to bypass.
Via `null byte`  `%00` to terminate the file path before the required extension.

**LAB** - `../../../etc/passwd%00.jpg`


#### Prevention

The best way is to not have user input passed into filesystem API.
But if that necessary, a two layer defense is recommended:
- Utilize white list for user input, alternatively verify that it contains only permitted content - alpha numeric characters.
- After validating the user input, append input to the base path and send it to the filesystem API to canonicalize, (shortest absolute path) - then verify that path starts with base dir.

Example java code.
```java
File file = new File(BASE_DIRECTORY, userInput);
if (file.getCanonicalPath().startsWith(BASE_DIRECTORY)) { 
	// process file
}
```