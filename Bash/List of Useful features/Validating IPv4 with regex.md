`^((25[0-5]|(2[0-4]|1\d|[1-9]|)\d)\.?\b){4}$`

Can be used to check if ipv4 address satisfies the regex.
``` bash
ip_addr="192.168.1.1" 
# Replace with your variable holding the IP address
if [[ $(echo "$ip_addr" | grep -E '^((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$') ]]; then 
echo "IP address validated"
fi
```
