WebSockets is a communication protocol:
- `event driven`
- `real-time`
- `bidirectional`

Provides a `persistent connection` between clients and servers.

Compared to HTTP which is `request-response` protocol.

Widely used in modern web apps.
Initiated over `HTTP` they provide long lived connections with asynchronous communication in both directions.

It fosters all kinds of user actions, data transmission.
Any web vulnerability that occurs in HTTP can also arise in relation to WebSockets communications.

**Asynchronous** - one end sends information, then there is a time lag until recipient takes information and responds.


#### Manipulating WebSocket traffic

Vulnerabilities in WebSockets usually involves manipulating them in ways the application doesn't expect.
Things you can try:
- `Intercept` & `modify` WebSocket messages.
- `Replay` & `generate` new WebSocket messages.
- `Manipulate` WebSocket connections.

This can be done with Burpsuite.
Some limited manipulation can also be done in browser web tools, console using JavaScript.


#### Intercepting and Modifying WebSocket messages

Using Burp Proxy:
- Open browser.
- Browse an app function that uses WebSockets, and monitor `WebSockets history` tab.
- Turn `intercept` ON.
- When the message is received, it will be visible in `intercept tab` to be modified and viewed.
- Press `forward` button to forward the message.

*Note*
You can configure whether client-to-server or server-to-client messages are intercepted in Burp Proxy. 
Do this in the Settings dialog, in the `WebSocket interception rules settings`.

#### Replaying and generating WebSocket messages

The process of replaying and generating messages is very similar to the one used for HTTP traffic.
Any message that we want to `resend` or `modify` can be sent to `repeater` (shift + R).

Lot's of that is similar to modifying HTTP traffic, and it's covered in note `Burpsuite`.

#### Manipulating WebSocket connections

It is sometimes necessary to manipulate WebSocket `handshake` that makes the connection.
Here are situations where modifying the handshake might be necessary:
- Increase `attack surface`.
- Attack might `drop your connection`, ergo reconnection is needed.
- `Stale tokens or data` from original handshake needs an update.

Modifying WebSocket handshake via Burp repeater:
- Get a WebSocket message into Burp repeater.
- In repeater, click on a `pencil` next to an URL.
- This prompts you to either - `attach`, `clone`, `reconnect` to a WebSocket.
- If you chose to `clone` or `reconnect` - you will see detailed `handshake request` - which you can edit before performing.
- Click `connect`, it will attempt the configured handshake - if the connection is successful you can use this to send new messages in Burp repeater.


#### Security vulnerabilities

In theory any web vulnerability can arise in WebSockets:
- User input could be processed unsafely - leading to `SQL` injections or `XML` external entity injection.
- Blind vulnerabilities reached thru WebSockets might only be detectable using `OAST` techniques. (out-of-band application security testing). Using external servers where otherwise vulnerability is invisible.
- If attacker controlled data is sent to other users, it may lead to `XSS` or other client-side vulnerabilities.


#### Manipulating WebSocket messages to exploit vulnerabilities 

Majority of input-based vulnerabilities in WebSocket messages can be exploited by tampering with their contents.
For example, we have a messaging web application.

It uses WebSockets to send messages between browsers and servers.
```json
{"message":"Hello Carlos"} # sent from you to server
```

And on the other end, the user renders the message. The `<td>` - table data - is an html tag and it defines a data cell in a table.
```html
<td>Hello Carlos</td> <!-- sent from server to recipients browser -->
```

In this example, given no input processing or defense is in place, we can send `PoC XSS` payload. (Proof of concept)
```json
{"message":"<img src=1 onerror='alert(1)'>"}
```

**LAB** - Use WebSocket messages to send an `alert()` pop up in agents browser, the messages are in a live chat and viewed in real-time.
If you are you using burpsuite community edition (the free one) you will not be able to intercept WebSocket messages, a great alternative is `Caido`.
It even supports more features that are limited or missing in burp.

#### Manipulating handshakes

Some design flaws can only be found in WebSocket handshake:
- `Trust in HTTP headers` - security decisions based on http headers such as `X-Forwarded-For`.
- `Flawed Session handling` - generally session context of a handshake will determine the context of WebSocket messages.
- `Custom HTTP header` - New attack surface introduced by a custom header.

**LAB** - Same as previous lab, but this one has an aggressive but flawed `XSS` protection. 

Definitively needed more XSS skills for this room.
Upon first XSS failure you get IP banned.
But that can be bypassed by using X-Forwarded-For and spoof your IP.
Then the obfuscated XSS passes the defense `<img src=1 oNeRrOr=alert 1>`.

#### Cross-site WebSockets

Some WebSocket vulnerabilities arise when an attacker makes a cross-domain WebSocket connection.
That connections origin would be an attackers website.

This is known as `cross-site WebSocket hijacking attack`.

It involves exploiting `CSRF` vulnerability on a WebSocket handshake.
This often has big impact and allowing privileged actions on behalf of the victim or data exfiltration from the user.

#### What is cross-site WebSocket hijacking

It arises when the WebSocket `handshake` is vulnerable to `CSRF`.
Meaning it relies only on the session cookie and there is no CSRF tokens or unpredictable values.

An attacker can create a malicious application that will handle WebSockets connections to the vulnerable application.
And it will be handled in the context of the user that is on the malicious application.

Then the attacker can send arbitrary messages and read the responses.
So unlike regular CSRF, this is a two way interaction with the compromised application.

#### Impact

Can enable the attacker to:
- Perform actions on the behalf of user.
- Retrieve users sensitive data.
- Wait for incoming messages containing sensitive data.

Depending on how to server-side is setup for WebSockets.
It can affect how much access and authorization the attacker will have over those two impacts.

#### Performing an attack

First we need to identify if the WebSocket handshake relies only on the user session-token.
And no other data, such as CSRF token or randomized or unpredictable values.

Example WebSocket handshake request that is likely vulnerable to hijacking:
```http
GET /chat HTTP/1.1
Host: normal-website.com
Sec-WebSocket-Version: 13
Sec-WebSocket-Key: wDqumtseNBJdhkihL6PW7w==
Connection: keep-alive, Upgrade
Cookie: session=KOsEJNuflw4Rd9BDNrVmvwBF9rEijeE2
Upgrade: websocket
```

*Note*
`Sec-WebSocket-Key` header contains is a random value for caching proxies.
It's not used for authentication or session handling.

This is just for allowing cached documents (JS, CSS) to be served from other servers to speed up loading and reduce bandwidth usage.

**LAB** - This shop has a live chat feature using WebSockets, use the exploit server to host html/JS payload to hijack and exfiltrate chat history the gain access to their account.
