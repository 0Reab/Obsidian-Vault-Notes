APIs in web apps works similarly to http request in a way that it returns data.
For example using GET request we can retrieve JSON raw data.

For example using curl and proxying request data to [[Burpsuite]].
```bash
curl -X PUT --proxy http://localhost:8080 https://catfact.ninja/breeds -k -d '{name:"cheese cat"}'
# PUT request
# others
# GET, POST, DELETE
```
This curl command attempts to use http put request to add json object.
(Might need foxyproxy browser addon for routing the request thru burp)

```bash
┌──(kali㉿kali)-[~]
└─$ curl -X POST -H "Content-Type: application/json" -d '{"username": "jeremy", "password": "cheesecake"}' http://localhost/labs/api/login.php

Output:
{"status":"success","token":"eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0=.eyJ1c2VyIjoiamVyZW15Iiwicm9sZSI6InN0YWZmIn0=."}
```
token made out of 3 parts comma separated.
header - body - signature is the form

Each section is base64 encoded.
In case where api is returning a token with missing signature aka last comma separated value.
This indicates we can tamper with that data possibly using jwt.io - website.

JWT - json web tokens


#### API recon

To start API testing we need to find out about it as much as possible.
Given this example:
```http
GET /api/books HTTP/1.1 Host: example.com
```
There might be a path `/api/books/mystery` by some logic.

And we need to find out the following:
- Input `data` - required and optional parameters for interaction.
- Types of `requests` - this involves HTTP methods and media formats.
- `Rate` limits.
- `Authentication` mechanisms.

#### API documentation

API's are usually publicly documented, especially when the API is intended to for external developers.
More often in human readable format, designed for developers to read and understand how to implement and use the API's.

Or in a machine readable format such as `XML` or `JSON` for automatic API integration and validation.

---

So it's a good start to recon the documentation.

Even if the API documentation isn't publicly available, you might be able to access it by some careful `web crawling`.
For example:
- `/api`
- `/swagger/index.html`
- `/openapi.json`

If you identify an endpoint for a resource, make sure you investigate the whole path.
Given this path you sh0uld explore the following `api/swagger/v1/users/123`:
- `/api/swagger/v1`
- `/api/swagger`
- `/api`

---

Additionally you can use a common API wordlist to recon.
But as usual be reasonable with rate limiting requests.

**LAB** - Exploit API endpoint using documentation. Delete user `carlos`.

*Solution*
Going to path `/api` displays a table of APIs and their usage and response codes.
With that information, heading to `/api/user/carlos`, leaks username and email.

But the goal is to delete the user, which is done by modifying that request with HTTP method `DELETE`.

#### Using machine readable documentation

There are many automated tools for reading and testing said APIs.
For example `OpenAPI Parser BApp`.
Or `Burp Scanner` for `OpenAPI` documentation or if it's in `JSON`/`YAML`.

And for testing, `Postman`/`SoapUI`.

#### Identifying API endpoints

You can find a lot of information by just browsing applications that use APIs.
This can be worth doing even if you have the `documentation`.
As sometimes it can be `inaccurate or outdated`.

Look for patterns in the URL that indicate use of APIs such as obvious ones `/api`.
Also look out for Javascript files, they could have references to endpoints you haven't triggered directly.

For more heavy duty extraction use `JS Link Finder BApp`.

#### Interacting with endpoints

Trying different requests and carefully examining the responses can lead you to discovering more attack surface and usage of said endpoint.
That includes, different `HTTP methods` and `media types`.
Often the `error` messages can be valuable to forge a `valid request`.

#### Identifying supported HTTP methods

The HTTP method specifies the action to be performed on a resource:
- `GET` - retrieve data from a resource.
- `PATCH` - partially alter a resource.
- `OPTIONS` - Retrieve allowed methods for a resource.

An endpoint may support different methods.
So it's important to test all potential methods to identify `additional functionality` and expand `attack surface`.

---

For example, `/api/tasks` may support:
- `GET /api/tasks` - retrieve a list of tasks.
- `POST /api/tasks` - create a new task.
- `DELETE /api/task/1` - delete a task.

*Note*
When testing API functionality, make sure that you are testing `low-priority` objects.
This avoids interfering with applications `functionality` and `data integrity` or creating excessive records.

#### Identifying supported content types

Endpoints expect data in a specific format.

Changing the format could:
- Cause errors that `disclose` useful `information`.
- `Bypass` flawed defense.
- Exploit different `processing logic` - it might process `JSON` safely but `fail` to do so with `XML`.

To change the content type, modify the `Content-Type` http header.
Then reformat the body according with new data type.
`BApp` can convert  between `XML`/`JSON`.

**LAB** - Exploit a hidden API endpoint to buy **Lightweight l33t Leather Jacket**.

**Solution**
The web app is has a list of items you can buy and a cart to purchase items.
Upon exploring the functionality in network traffic we notice triggered API endpoint.
`/api/products/1/price`

When using GET request, it fetches the item price.
We explore other HTTP methods on the API.
Only the `PATCH` method works, but...
We need to be an authenticated user, otherwise we get "Authorization Error".

And upon logging in as a user, and requesting that endpoint again with patch method, we get "incorrect Content-Type" error.
Based on that, request is updated to have `Content-Type` header, just below `User-Agent` not sure if order matters.
And again, now we get "missing parameter price in JSON".

Based on this, we can patch the price of product 1 by providing correct JSON, `{"price": 0}`.
Remember JSON is a data structure that only has to be valid and for displaying's sake it's not crucial to have tabs and whatnot.
Just use double quotation marks  and valid brackets.

Now we can buy this product at a great price of $0.00.

#### Finding hidden endpoints

Given an path to endpoint of `PUT /api/user/update`.
We can try and replace `update` with some logical functionality like `add`/`delete`.

When looking for hidden endpoints use wordlists based on common API naming conventions and industry naming standards.
And as mentioned add terms relevant to the application's functionality and your recon.

#### Finding hidden parameters

While doing API recon you might find undocumented parameters.
- Burp intruder allows you to use a wordlist of common parameter names to add/replace parameters, make sure to use names that are relevant to your application.
- `The Param miner BApp` can test with 65,536 parameters per one request. It automatically guesses names that are relevant based on info from scope.
- Content discovery tool, enables you to discover that isn't linked to visible content that you can browse to, including parameters.

#### Mass assignment vulnerabilities

Auto-binding can cause hidden parameters.
It happens when framework binds parameters to fields on an internal object.
Mass assignment, can support parameters that were never intended to be processed by the developer.

#### Identifying hidden parameters

Because mass assignment creates parameters from objects (JSON).
We can `derive` the `parameters` from objects returned by API.

For example `PATCH /api/users/` updates the username and email with the following JSON.
```json
{
	"username": "wiener",
	"email": "wiener@example.com"
}
```

And `GET /api/users/123` returns:
```json
{
	"id": 123,
	"name": "John Doe",
	"email": "john@example.com",
	"isAdmin": "false" 
}
```

This may indicate that the parameters `id` and `isAdmin` are bound to internal user object alongside the updated username and email parameters.

#### Testing mass assignment vulnerability

As previously discovered user object has id and admin value.
We can try and add the admin parameter to our `PATCH` request and see how the application behaves with valid data such as "false" and invalid data like "foo".

If we cause no errors with "false", and errors show up with "foo" it may be possible to allow our user to become and admin.
We will resend the request to update our username/email, and modify the API request.
`PATCH /api/users/`
```json
{
	"username": "wiener",
	"email": "wiener@example.com",
	"isAdmin": "true"
}
```

Via GET request we can verify if the object updated and test our privileges and access.

**LAB** - Exploit mass assignment vulnerability to buy Lightweight l33t Leather Jacket.

Solution -  Update the product object's discount percentage value by using POST method on the API.

#### Preventing vulnerabilities in APIs

When designing APIs make sure to:
- Secure the documentation if the API is not public.
- Ensure the documentation is up to date for your testers.
- Apply a whitelist of allowed HTTP methods.
- Validate the content type of each request/response is expected.
- Use generic error messages to not give away information.
- Use protective measure on all API versions not just for current.

To prevent mass assignment vulnerability use a allowlist for properties that users can edit and blocklist for sensitive properties.

#### Server-side parameter pollution

On some applications that utilize internal APIs we can try and interact with them by polluting our requests.
This vulnerability occurs when the application embeds `user input` in a request to an `internal API` without adequate encoding.
This means that the attacker could manipulate or inject parameters.

This could lead to:
- Override existing parameters.
- Modify the application behavior.
- Access unauthorized data.

`Any user input` can be used to test this vulnerability.
- query parameters
- form fields
- headers
- URL path parameters

*Note*
This vulnerability is sometimes called HTTP parameter pollution, but this is also the name of a WAF bypass technique.
And there is server-side prototype pollution which sounds similar but it's not related to our vulnerability.
So for clarity's sake we are going to refer to current vuln as `server-side parameter pollution`.

#### Testing server-side parameter pollution - query string

To test query string we can inject characters such as `&`, `=`, `#` and observe the response.

For example, given an app the searches for users based on username.
Upon searching the following request is sent.
```http
GET /userSearch?name=peter&back=/home
```

To return the information, the server requests data from internal API with:
```http
GET /users/search?name=peter&publicProfile=true
```

#### Truncating query strings

You can attempt to truncate the server-side request using URL encoded `#`.
For ease of parsing of the response, you can also add a string after it.

For example here is the modified URL that you request:
```http
GET /userSearch?name=peter%23foo&back=/home
```

And the front-end will access this URL:
```http
GET /users/search?name=peter#foo&publicProfile=true
```

*Note*
It is necessary to URL encode the #.
Otherwise the front-end will interpret it as a `fragment identifier`. 
And it won't be passed in to internal API.

**Fragment identifier** - in short as fragid, is used in URI that denotes part of the document to retrieve like header or else.
URI vs URL - uniform resource identifier vs uniform resource locator.
The former concerns of type and latter concerns of location.

The response of truncated request can indicate if truncation was successful.

If we receive an `error`, it is likely that the application treated "foo" as part of a name.
If we receive the `name`, it's likely that the truncation was successful.

So if we are able to truncate the server-side request, it could be possible to lookup the non public users by truncating that parameter.

#### Injecting invalid parameters

You can use `&` character to try and add another parameter to the request.

For example:
```http
GET /userSearch?name=peter%26foo=xyz&back=/home
```

Then the server-side API:
```http
GET /users/search?name=peter&foo=xyz&publicProfile=true
```

And observe the response.
If the response is unchanged, it could indicate that the app simply ignored the parameter.
It is necessary to test further to paint the whole picture.

#### Injecting valid parameters

If you are able to modify the query string, you can attempt to add a valid parameter to the server-side request.
*Related section - Finding hidden parameters*

For example, if you've found email parameter:
```http
GET /userSearch?name=peter%26email=foo&back=/home
```

And the server-side API:
```http
GET /users/search?name=peter&email=foo&publicProfile=true
```

Review the response.

#### Overriding existing parameters

To confirm if the app is vulnerable to server-side parameter pollution we can try to override the `original parameter`.

For example, we can try and duplicate the parameter:
```http
GET /userSearch?name=peter%26name=carlos&back=/home
```

And for the internal:
```http
GET /users/search?name=peter&name=carlos&publicProfile=true
```

Now the two parameters, both called `name` can be interpreted differently depending on the application technologies:
- `PHP` - would take the last parameter - `carlos`.
- `ASP.NET` - would combine both parameters - `peter,carlos` - likely causing an error. (As most `.net` things this Microsoft's web framework).
- `Node.js` - would take the first parameter - `peter`.

If you are able to override the parameter, you can try setting your name as `administrator`.
This may allow you to `log in` as the administrator user. 

**LAB** - Exploit server side parameter pollution in query string. Log in as administrator and delete user Carlos.

---
*Solution and rant*
Okay, hate to admit it but I'm an idiot.
As mentioned multiple times, query strings need to be URL encoded to work.
I assumed that the dev tools automatically URL encode http body because as I first looked at the request "&" and "=" were not encoded.
So I figured it encodes upon sending it.
Big blunder.

Next time, no assumptions for such a critical part, better redundant and safe than being as deadly as a butter knife.

After fiddling with the query strings of login and password reset functionality, and failing to see any meaningful way forward based on error messages.
What I definitively missed is, password reset JS file that had some crucial information.

You can have syntax highlighting in the browser dev tools in "Debugger" section and browse thru all the scripts.

In that script, there is a mention of a path with token parameter.
```javascript
window.location.href = `/forgot-password?reset_token=${resetToken}`;
```

Visiting this URL leads to a password reset for the account of a given token.
This token can be obtained buy injecting parameters in the reset password post request.
As the body contains:
`csrf-token&username=administrator`
This returns some data, such as hidden email.

After trying to append parameters and truncate them with `& and #` the errors seem to indicate there are `fields` that we can specify.
This request `csrftoken&username=administrator#test` truncates the parameters and causes:
```json
{"error": "Field not specified."}
````
And for the language barrier, ''field" would mean literal 'field' as a parameter name.
Here is the bruteforcing moment, or if you thing a bit, we can utilize our recon skills and try things such as:
- `username`
- `email`
- `reset_token`

All of those parameters returned their respective data when assigned to `field`.
And the token can be used to reset the password and complete the lab.
`csrf-token&username=administrator&field=reset_token`

```json
{
	"type": "ClientError",
	"code": 400,
	"error": "Invalid field."
}
```

Okay and to summarize.
Using both `#` and `&` you can manage to enumerate the internal API.
When using `#` to truncate and cause errors getting the name of the `next parameter`.
Then using `&` to enumerate the values you could assign to the `filed` parameter (email, username, reset_token).
Using your good recon you manage to find the reset token and exfiltrate the token via internal API.
And simply sending a request on that URL you found in the same script as token, you send a request with the token and boom.
N1 h4ck3rm4n!

---

#### Testing server-side parameter pollution in REST paths

A REST API may place parameters in the `URL path` rather than the query string.
`/api/users/123`

The path can be broken down into:
- `api` - root of endpoint.
- `users` - resource - in this case users.
- `123` - parameter - specific user.

For example, an application where you can edit your profile based on your username.
`GET /edit_profile.php?name=peter`

Resulting in this server-side API request:
`GET /api/private/users/peter`

An attacker might be able to alter the URL path parameters.
As we now how to exploit paths, we can utilize path traversal.
URL encoded `peter/../admin` will end up as name parameter.
`GET /edit_profile.php?name=peter%2f..%2fadmin`

And the server-side.
`GET /api/private/users/peter/../admin`

If the server-side client or the API normalize this path it may be resolved to
`/api/private/users/admin`.

#### Testing server-side parameter pollution in structured data formats

As servers use data formats such as `JSON` & `XML`.
We can attempt to exploit them by injecting data that is structured according to the formats.

Consider an application that allows you to `edit your profile`.
The changes are applied via `server-side API`.
When you edit your name the browser makes the `request`:
```http
POST /myaccount
name=peter
```

Then the server-side API request:
```http
PATCH /users/7312/update
{"name":"peter"}
```

Let's attempt to add `access_level` parameter to our browser request.
```http
POST /myaccount
name=peter","access_level":"administrator
```

If this data is added to the server without input sanitization and validation of JSON data.
It will result in the following server-side request.
```http
PATCH /users/7312/update
{"name":"peter","access_level":"administrator"}
```