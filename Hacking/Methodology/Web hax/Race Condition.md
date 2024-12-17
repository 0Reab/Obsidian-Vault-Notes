As the name suggests *race* condition is a type of vulnerability which involves time.
```python
 if user['balance'] >= amount:
        conn.execute('UPDATE users SET balance = balance + ? WHERE account_number = ?', 
                     (amount, target_account_number))
        conn.commit()

        conn.execute('UPDATE users SET balance = balance - ? WHERE account_number = ?', 
                     (amount, session['user']))
        conn.commit()
```

In this code example conn.execute() function has SQL query as a parameter.
Meaning that every time user sends or receives funds it will take some time for the server to process this *UPDATE* to the database.

The first if statement check we cannot really tamper with but as the rest of the if block execution is time dependent.
We can attempt to exploit the race condition and send http requests in quick succession, to try and trick the system.

Essentially what is happening is the requests that we spam to the server are faster than the server can process them.

## Practical

This can be achieved with two methods.

Sending requests in parallel (*parallel*)
Sending requests in parallel (*last-byte sync*)

I will refer to these methods as they are described in brackets ().

In burp suite we should capture a request that is going to let's say transfer funds as our example is related to balance.
We can duplicate this request via ctrl + r some number of times, eg. 9.

And group them together, now this group of requests can be sent together in *parallel*.
(Which is still pretty fast in regards to time succession).

Or the *last-byte sync* method which will in a nutshell send all of the requests but not completely, now the server won't start processing incomplete requests.
And we will use this fact to our advantage and send last bytes of the requests to '*push*' them thru at almost same time.

The server if improperly secure, will have problems processing these requests and you can end up sending greater funds than your account balance.
For example user A is sending 100 to user B. (this request is spammed 10 times).

Before:
A - funds 500
B - funds 0

After:
A - funds -500
B - funds 1000