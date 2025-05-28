Are similar to HTTP requests but the difference is that Web Sockets keep the connection alive to exchange data more easily.
So in some sense they are similar to one another.

But importantly the difference in security.

- **Weak authentication and authorization**:
	- Unlike http, web socks don't have built it way to handle auth and session validation.
	- If this is not secured properly attacker could access sensitive data or mess with connection.
- **Message tampering**:
	- If encryption isn't used attacker could intercept and change messages.
	- Injecting commands, perform actions, or alter data.
- **Cross-Site WebSocket Hijacking (CSWSH)**:
	- This is when attacker tricks users browser into opening web sock connection to a site.
	- If successful, attacker might be able to hijack the connection or access data meant for legit server.
- **Denial of Service (DoS)**:
	- Because web sock connections stay open, they can be DoS-ed.
	- An attacker could flood the server with messages thus slowing it down or crashing it.

Web socks are great for real-time apps such 