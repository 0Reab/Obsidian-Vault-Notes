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

