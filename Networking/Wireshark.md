Handy packet filter list:
-  tcp.len > 0  # filters out tcp handshakes (decluttering).
-  tcp.port == 22 # filter for port number x
-  and # logical gate for combining filters

Below packet filter there is an input field for display filter for matching strings, regex etc. within packets.

## Searching for file transfers

Statistics > Conversations (4th row) >  TCP tab

Sort by bytes to get an idea of data transfers.

