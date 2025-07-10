Use the `Server-Side vulenrabilities` note for reference.

This practical labs for `Access control` only.


**LAB** - Delete user Carlos from admin panel by changing your  accounts `roleid` to `2` to gain access.

The URL endpoint for changing email address is used to modify your `account data`.
Ergo it's suitable to modify our `ID` if access control is not secure.

`POST /my-account/change-email`
```js
{ "email": "BOOMBA@mail.com" } // post data
```
`Response:`
```js
{
    "username": "wiener",
    "email": "BOOMBA@mail.com",
    "apikey": "redacted",
    "roleid": 1
}
```

Based on this response we can conclude the data server uses to define users.
So we can modify our `POST` request to include `roleid` in the `JSON` and see if manage to modify our `ID`.

`POST /my-account/change-email`
```js
{
	"email": "BOOMBA@mail.com",
	"roleid": 2 // new post data
}
```
`Response:`
```js
{
    "username": "wiener",
    "email": "BOOMBA@mail.com",
    "apikey": "redacted",
    "roleid": 2 // id updated to 2
}
```

We now have access to admin panel and after deleting the user Carlos the lab is solved.


**LAB** - horizontal privilege escalation to another user API key on the user account page.
**Solution** - Bruh solved it in 3 seconds... After you login as Peter on  your page `/my-account?id=wiener` just change your id param to `carlos` and the API key updates to carlos.


**LAB** - Data leakage in redirect and user ID controlled by request parameter. Obtain another users API key. 
**Solution** - `GET /my-account?id=carlos` endpoint is vulnerable to access control bugs and because the profile page after you authenticate contains you API key.

You can leak the API key of other users by changing the `ID` parameter and capture the request to see the `API key`.
Which would otherwise be easy to miss because the page redirects you to `/login` immediately.
So this is  a case of "cosmetic security" if I were to give it a descriptive name.


**LAB** - Find Carlos password, the server uses static URLs and writes and fetches the chat logs on it's filesystem.
**Solution** - Intercept `POST` request for `/download-script` it's found on a button to view transcript.
After following the redirect, you end up on `GET /download-transcript/3.txt` from where you can pick a text file to fetch, helps that they have predictable names.


**LAB** - Access the admin panel to delete an user. The panel is accessible to any user but it's restricted from external access. You can bypass it using a header `X-Original-URL`.
**Solution** - As the admin panel responds with access denied, we will utilize this non standard header to trick the backed into believing we are not accessing it from external source. 

If we do a basic request at the root `/` and add the header with value `/admin` the request to admin will be successful.

*Note:* keep in mind every request will have to be sent to an endpoint/path you can already access normally best use root `/`, and then include the header with the path you need to access.
Also any parameters that you want in the URL will need to be appended to the request path (not the header path).
The final request will concatenate the URL path + header + URL parameters.

If this last bit was a bit confusing this example will demonstrate it.

After accessing the admin panel you will see the endpoint for deleting a user.
`/admin/delete?username=carlos`

So to craft the next request we will be fetching the path - `/?username=carlos`, even though it's not a real path in this form.
And the header value set as path - `/admin/delete`.
The two URLs will be concatenated.`/admin/delete?username=carlos`


