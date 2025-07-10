Many things on the webpages use templates.
For example if we have a web page that displays a user profile.

It updates  the necessary information that will change based on the user.
So in a user profile, changing their name, bio etc. can be done via template while rest of the page remains the same or adapts to the template.

```js
user = "Username: {{ user.name }}";
template.render();
```

Here is an example of template injection.
```js
bio = "Bio: " + user_bio;
tempalte.render();
```


The `user_bio` variable if not validated and filtered could take advantage of the template system and inject payloads:
- `{{ 7 * 7 }}` - 49
- `{{ exec('ls') }}` - *files list here*

It's worth clarifying, as the name of vuln is server-side ... this template injection can reflect anywhere, meaning it can reflect client side too.

Now in a case where backend is a `Flask` server in python.
And there is `SSTI` vulnerability.
It is possible, depending on framework for templates to import modules such as `os` module, then RCE is on the table.

But if the framework does not allow for importing of modules.
Python interpreter itself utilizes python to run.
Meaning the `os` module could be at reach.

Or other already used modules in the web app already use `os` in their source.
So it's just a matter of finding the right one by using `gadgets` to achieve full RCE.

Example research: `ssti python gadgets`


**LAB** - Lab uses ERB template (Ruby) and it's vulnerable to template injection.
**Solution** - the web app only has little interaction available and the first item reports error out of stock when clicked on.

No pace accepts user input, which should've guided me to that one item but whatever I'll remember for the next time.
Upon that error message, the GET request is executed with a message http parameter.
This can often be harder to find if you are not used to it but it can be a forgotten vector to many injection vulnerabilities.

In this case after we identify the ERB template is vulnerable to injection by testing some multiplication.
The final payload is delivered to complete the lab by deleting a text file.

/?message=%3C%=%20`rm+morale.txt%60%20%%3E


