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

