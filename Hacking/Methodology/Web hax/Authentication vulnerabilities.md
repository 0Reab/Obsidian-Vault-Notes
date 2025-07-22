### What is authentication?

Is a process of verifying the identity of a user or client.

This is a crucial part of the web security because websites are exposed almost everyone has access to the internet.

Three main types of identification:
- `Knowledge factors` - Something you remember, such as password.
- `Possession factors` - Something you own, such as USB key.
- `Inheritance factors` - Something you are or do, such as biometrics.

### What is the difference between authentication and authorization?

As mentioned authentication is concerned with *who* you are,
 and authorization is concerned with *what* you are allowed access.

For example, once you are authenticated as a user, 
you are now authorized to edit your user account details.

### How do authentication vulnerabilities arise

Most common ways:
- Fail to adequately protect against `brute-force` attacks, such as rate-limiting.
- `Logic flaws`, bad coding implementation can lead to auth bypass, often called "broken authentication".

### Impact of authentication vulnerability

Often is *critical*, the reason being in worst case scenario the attacker can compromise the administrator account and possibly escalate that into an application or server or internal network compromise.
In less critical scenario, all user accounts can be compromised and their personal information can be leaked, or unwanted actions can be taken on their behalf.

### Vulnerabilities in password-based login

In this scenario we have an authentication that works with username and password.
So getting the password will result in account takeover, or leveraging some other vuln.
We will cover brute-forcing and basic http authentication.

For such web apps with password only authentication can be vulnerable to brute-forcing if proper rate-limiting is not in place.
This process involves usually a `wordlist` or a finetuned one based on recon.

More often than not, web apps `disclose username` either intentionally or not, for example username can be the same as profile name.
Or the email is disclosed in some other place.

The `password policy` can have a great impact on their security, such as requirements for length, special characters, and numbers and capitalization.

The biggest weakness of password policy is the human factor, meaning that if a person wanted to set their password as `password`.
The policy would force them into adding all the required complexity, which could result into a predictable `Password1!'.

### Username enumeration

This can be done by attempting to login with invalid password.
The web app will notify you that the password is incorrect thus validating our claim of the username existence.

When brute-forcing a login page, pay attention to the following:
- `Status codes`
- `Error messages`
- `Response times`

All of the listed items could indicate of successful enumeration.
And thus they should not be set up in a way to disclose information in those manners.


**LAB**: Username enumeration via different responses

**Solution**: In the login POST request `username=ap&password=mobilemail` we enumerate the username first.
And based on response length we see that the `ap` username returns `invalid password` instead of `invalid username`.

Confirming we have a valid username, repeat the process for the password and find a successful request based on length.
Successful requests response ended up being the shortest, because it was a 302 redirect. 


**LAB**: Username enumeration via slightly different responses

**Solution**: In the login POST request `username=app1&password=batman` we enumerate the username first.
And based on response length on each request is different and with a message `invalid username or password.`
But one response has `no` comma in in the message `invalid username or password`.
We have the username now just  brute-force the password and find 302 response instead of 200 like every other.


**LAB**: Username enumeration via response timing

**Solution**: The main idea in response timing brute-forcing is to make the server processing time longer by increasing our payload size.
So in our case, if a server detects an invalid username it immediately stops and responds with `fail`.

But if we enter a valid username but an `invalid password of 100 characters`, then the server will attempt to match the valid username and it's password to our supplied password.
Thus `increasing the processing time` because we inserted such a long password.
Now this is in matter of milliseconds, so `200 ms` average request and `500 ms` for our successful enumeration.

In this instance the server allows around 3 failed login request and you get 30 min timeout.
To bypass this brute-force defense we add `X-Forwarded-For: <ip>` header with whatever IP address which we can increment any octet every request (for simplicity sake).

So eventually one username returns `500 ms` response round trip time and we get 302 response code on successful login `username=ftp&password=thunder`.

#### Flawed brute force protection

Two most common ways of preventing brute-force attacks:
1. `Locking the account` - the user failed to log into multiple times.
2. `IP blocking` - if they make too many login attempts in quick succession.

Both approaches offer varying degrees of protection, but neither is invulnerable.
Especially if implemented with a flawed logic.

For example, in a scenario where the user's IP is blocked if they fail to login too many times.
To bypass this protection, we can login into our account to reset the `fail` counter and continue to brute-force.
So this defense type could end up completely useless.


**LAB**: Broken brute-force protection, IP block
- Your credentials: `wiener:peter`
- Victim's username: `carlos`

**Solution** - `carlos:access`
To bypass the flawed brute-force protection we need every 3rd request to be successful.
So every 3rd entry in our username list and password list needs to be our login creds.
To craft the wordlists.

```bash
#!/bin/bash

# temp script to insert usrename every n-th line in wordlist

result="$HOME/final_wl.txt"
wordlist="$HOME/wl.txt"
pass="peter"

count=0

while read -r line; do 
	((count++))	

	if [ "$count" -eq 3 ]; then
		echo "$pass" >> "$result"
		echo "$pass"
		count=0

	else
		echo "$line" >> "$result"
		echo "$line"
	fi

done < "$wordlist"
```

I made this script to insert `peter` (our username) every 3rd line while parsing an actual username wordlist.
I made this script to insert `wiener` (our password) every 3rd line while parsing an actual password wordlist.
```bash
#!/bin/bash

# temp script to insert usrename every n-th line in wordlist

#wordlist="$HOME/wl.txt"
my_user="wiener"
target_user="carlos"
result="$HOME/final_users.txt"

count=0
loop_iter=0

while true; do 
	((count++))	
	((loop_iter++))	

	if [ "$count" -eq 3 ]; then
		echo "$my_user" >> "$result"
		echo "$my_user"
		count=0

	else
		echo "$target_user" >> "$result"
		echo "$target_user"
	fi

	if [[ "$loop_iter" -eq 99 ]]; then
		break
	fi
done
```

#### Account locking

Some web apps might try to defend against brute-forcing login by `locking accounts`.

So just as with normal login errors, this can help us `enumerate usernames`.
Locking an account offers some sort of protection, but if the attacker is not targeting a specific user, it will not prevent mass brute-force (not specific account targeted).

Therefore an attacker can target any account.

This is the methodology:
1. `Username wordlist` - this can be by enumeration or a generic common username wordlist.
2. `Small password wordlist` - the length of this wordlist needs to be equal to allowed login attempts.
3. `Use automated tool` - brute-force each username with password combinations, without locking out users. (you only need to compromise one user).

So in short, we are aiming for `weak combination` of username & password.


Account locking also fails to protect against `credential stuffing attacks`.

This for example is when an attacker utilizes a known big wordlist of `username:password` combinations found in previous data breaches/leaks.
So it does not matter if an account lock out is in place because every attempt is going to be for a `different account`.
Many accounts can be compromised via this method.


**LAB**: Username enumeration via account lock
Enumerate usernames and then brute-force the password.
The web app has a account lock out if 3 failed login attempts.

**Solution**: I basically ran the small wordlist multiple times until I noticed one requests response length is different.
It had a message saying the user is locked out for a minute.

Next I thought, well what's the point if I'm going to get locked out for a minute each 3 failed attempts.
But I guess the idea is since we are aiming for very weak creds combination, this is how it goes.
And low and behold first password in the wordlist was a hit.
`af:123456`

#### User rate limiting

Making too many login attempts can lead to an `IP ban`. 
This ban is usually temporary and can be resolved by one or more of the following:
1. `Waiting`
2. `Solving a captcha`
3. `By administrator`

Rate limiting is preferred over `account locking` due to DoS attacks and username enumeration.

However it's not bulletproof.
There are ways to manipulate their `apparent IP` address.

Or it can be bypassed if it's possible to guess multiple passwords with a `single request`.

###### HTTP basic authentication

Although fairly old, it's simplicity and ease of implementation means you might see HTTP basic authentication being used.

It works like this:
The client sends an authentication token.
The token is made out of `username:password` encoded in `base64`.

This token is stored and managed by the server.
It automatically adds the token to every subsequent request as follows:

`Authorization: Basic base64(username:password)`

For many reasons this is not considered a safe authentication method.
First, it involves sending user credentials with every request.

This is a problem if the web app does not implement `HSTS` (forcing use of https aka TLS/SSL encryption).
That means if an `MITM attack` (man in the middle) is performed the credentials are basically plain text. 

Additionally HTTP basic authentication often doesn't support brute-force protection.
As the token consists of `username:password` in base64 this can it vulnerable to being brute-forced.

Also particularly vulnerably to `session-related` exploits, such as `CSRF`, against which it doesn't provide protection on it's own.

In some cases exploiting this authentication might only lead to access of uninteresting page.
Regardless this can lead to `larger attack surface` and the credentials could be potentially reused in a more `sensitive context`.

#### Vulnerabilities in multi-factor authentication

Many websites rely on `single-factor` authentication using a `password` to authenticate users.
But some require `multi-factor` authentication so users can prove their identity.

Verify biometric factors is impractical for most websites.
But it is increasingly common to encounter `two-factor` authentication `(2FA)` based on `something you know` and `something you have`.

This requires users to enter their traditional `password` and a `temporary verification code` from an out-of-bound physical device in their possession.

2FA is more secure than a single-factor method such as password only.
However it is as secure as it's implementation, it can be beaten or `bypassed entirely` just as a single-factor method can.

Worth noting.
Most of the benefits of a multi-factor authentication are only achieved by verifying multiple `different` factors.
Verifying the `same` factor in different ways is `not a true two-factor` authentication.

`Email` based 2FA is one such example.
Although the user has to provide the password and the email verification code, accessing the code only relies on them knowing the credentials of the email account.
Therefore the knowledge authentication factor is being verified twice.

#### Two-factor authentication tokens

Verification codes are usually read by the user from a physical device.

Some high security websites now provide a dedicated device for this purpose.
Such as a `RSA token` or `keypad device`.

In addition to being purpose built for security, these devices have the advantage of `generating the verification code directly`.
Alternatively websites use `Google Authenticator`, for the same reason.

On the flip side, some websites send the verification codes to the users phone as a text message (SMS).
While this is still technically something `you own`, it is open to abuse.

The reason being, the code is not being generated directly on the device, rather it is being transmitted to the device.
Which opens an attack surface, for attacks such as `SIM swapping`, or simply intercepting the SMS message.

The intercepting is done via `stingray` which acts as a transmitter tower and allows you to intercept SMS messages, which are otherwise encrypted `over the air`,
but not `end-to-end` encrypted.

And the SIM swapping is done by stealing you phone number for use, by means of social engineering or other methods. 
And thus the attacker can receive the SMS messages.

https://portswigger.net/web-security/learning-paths/authentication-vulnerabilities/vulnerabilities-in-multi-factor-authentication/authentication/multi-factor/bypassing-two-factor-authentication