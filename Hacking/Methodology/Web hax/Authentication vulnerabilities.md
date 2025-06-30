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

