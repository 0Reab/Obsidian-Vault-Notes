This protocol serves the purpose of transferring files.
It works on [[TCP]] and data is in plain-text.

Meaning there is no encryption in place for security.

This protocol is on port 21 and 22.
21 - data such as commands.
22 - is for file transfers.

FTP server supports either of two types of connections.

Active - the client opens the port and listens, the server is connecting.
Passive - the server opens the port and listens, the client is connecting.

