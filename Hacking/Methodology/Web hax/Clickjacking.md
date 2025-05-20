
Is a interface based attack, where user is `tricked` into clicking on a obfuscated prompt to perform a hidden and unwanted action.

This attack is somewhat similar to `CSRF` attack.
But the difference is that clickjacking requires the `user interaction` such as clicking on a hidden button, compared to forging a whole new request like in CSRF without any user interaction.

For example you receive and `email` saying you won 10 gazillion dollars.
You click the `link` and there is a button to claim your prize.
Hidden underneath that is a button that is from another web app to transfer your money to attackers account.

That button you just clicked is within an `iframe` which is overlaid on top of the button you are actually clicking.


`iframe` - inline frame is an html element that allows you to embed another document into the current HTML.

```html
<!DOCTYPE html>
<html>
<body>
<iframe src="https://www.w3schools.com" title="W3Schools Free Online Web Tutorials"></iframe>
</body>
</html>
```
In this example the iframe is a little window rendering what the URL as if you visited it normally.

As we learned in CSRF note, CSRF tokens are used to prevent forged requests from going thru via token or same origin headers and else.
In this case with clickjacking non of that matters because the request happening on the `actual domain`.
The request looks all like normal behavior except the part that process occurs within an `iframe`.


#### Basic clickjacking attack

Clickjacking utilizes `CSS` to create and manipulate layers.
The target website is incorporated into an iframe overlaid on top of a decoy website.

In other words we "steal" a button from a legit website, stick it into our decoy to trick the user into clicking it.
```html
<head> 
<style>
#target_website 
{ position:relative; width:128px; height:128px; opacity:0.00001; z-index:2; }

#decoy_website
{ position:absolute; width:300px; height:400px; z-index:1; }
</style>
</head> ...
<body>
<div id="decoy_website"> ...decoy web content here... </div> <iframe id="target_website" src="https://vulnerable-website.com"> </iframe>
</body>
```

As you can tell by the CSS settings.
The `relative and absolute position` are set to achieve proper overlap.
And `opacity` is set to basically invisible value of `0.00001` which if set to `0.0` could trigger browsers anti clickjacking protection, depending on browser and version.
Also `z-index`  is a and indicator of z axis, what should be on top, the higher the number the higher the item is on top.

**LAB** - craft some HTML that frames the account page and fools the user into deleting their account.

```html
<style>
    iframe {
        position:relative;
        width:200px;
        height: 600px;
        opacity: 0.0001;
        z-index: 2;
    }
    div {
        position:absolute;
        top:495px;
        left:75px;
        z-index: 1;
    }
</style>
<div>Click me!!!</div>
<iframe src="https://0a190054039c9af780470dd8002600bc.web-security-academy.net/my-account"></iframe>
```

So we have an iframe and a div.
They are stylized with CSS, iframe is positioned relatively and has got a z-index higher than div underneath it.
Div is placed with lower z-index value and is just a "fake button" that is positioned where the iframe button appears to be.

As this process of manually creating a clickjacking page can be tedious.
There are tools to help with that, one of them is `Clickbandit` in burpsuite.

**Clickjacking prefilled form input**

Some web apps require filling in forms using GET requests before form submission.
As GET values form part of the URL, the URL can be modified to attackers preference.

**LAB** - Make the user change their email by clickjacking with prefilled forms.
`web-security-academy.net/my-account?id=wiener&email=hombre@user.net`

We can use the prefill in GET request to setup an email we want to use upon clicking the change email button.
It wasn't explicitly fetching GET request with email parameter, but we crafted it and it works.

*In case you are having trouble with any labs, try putting "Click me!!!" in the div*

#### Frame busting scripts

Because clickjacking is possible if the website can be framed.
Here are some measures that are taken to prevent framing.

These protections are `client-side` and work within the `browser`.
They are intended to break the scripts and detect frames or when a website is not the topmost window.

- check if the current app is the top most window.
- make all frames visible.
- prevent clicking on invisible frames.
- intercept and flag potential clickjacking 

Because these techniques of defense are browser & platform specific, they can be circumvented.
Since frame busters are JS, either browsers security might prevent them from running or lack JS support altogether.

One bypass technique is to use `HTML5 sandbox` attribute.
When it's set with:
- `allow-forms` (added)
- `allow-scripts` (added)
- `allow-top-navigation` (omitted)

Then the frame buster defense is ineffective because the iframe cannot check if it's a top window or not.

```html
<iframe id="victim_website" src="https://victim-website.com" sandbox="allow-forms"></iframe>
```

Bot `allow-forms` & `allow-scripts` allow those actions in the iframe.
But top-level navigation is disabled, this allows the functionality within the targeted site while still bypassing frame busting. 

**LAB** - Clickjack the user to change the email, website is protected by frame buster.

Solved like the previous lab by using a GET parameter with prefilled form for the email, and just added `sandbox="allow-forms` into the iframe tag, to bypass frame busting.

#### Clickjacking with a DOM XSS attack

Clickjacking has been historically used for boosting likes on a social media post for example.
But it can be used in tandem with other vulnerabilities for greater impact.

If XSS vulnerability has been discovered and clickjacking is possible, it can be combined to execute XSS when present in iframe.

`XSS` - cross site scripting (injecting JS into input fields causing it to ex)
`DOM` - Document Object Model - (JavaScript object) (API for HTML and XML) Web browser presents structure of a document (dynamically loaded and what else) (tree data structure).

Therefore DOM XSS works by modifying the DOM environment in your browser.
So client-side environment runs in an "unexpected" way.


**LAB** - This lab contains an XSS vuln triggered by a click, make a clickjacking decoy to click the button to call the print()

`https://.web-security-academy.net/feedback?name=%3Cimg%20src=1%20onerror=print()%3E&email=test@gmail.com&subject=what&message=omg`

This payload has been constructed by observing an feedback form, using it we formulate a `GET` request URL using the form fields.
The field `name` contains an broken image html tag with JS, it is executed upon form submission.

This can be `discovered` by trying to insert HTML elements such as `H1` (header) tags into the filed and submitting the form.
H1 is rendered as such upon submitting.

#### Multistep clickjacking

In some cases the attacker might need the user to perform multiple actions to achieve the goal.
For example if you are ordering something from a store, you need to add an item to the cart first.

These actions can be implemented by using multiple divisions or iframes (`<div> & <iframe>`), they require precision to be effective and stealthy.
So basically, stacking div elements and iframes to make more user actions possible.

