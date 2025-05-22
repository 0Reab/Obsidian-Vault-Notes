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

