Used for displaying active connections to your machine.

Netstat's output is separated in columns by:
   Protocols (TCP or UDP)
   Local address and port number separated by ":"
   Foreign address (IP address of device you are connected to)
   Status (established, closed_wait, listening)

Helpful for troubleshooting and monitoring applications for security purposes.

Additionally useful switch "-ano" that consists of 3 switches is commonly used.
- **-a**: This switch shows all connections and listening ports.
    
- **-n**: This switch displays addresses and port numbers in numerical form, rather than hostnames and service names.
    
- **-o**: This switch displays process ID related to each connection.
Therefore "-ano" adds 5th column for PID (process identifier).