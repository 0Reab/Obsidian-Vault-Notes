Defining a scope in burp.
Reaching a url thru a burp proxy and right click to add to scope the main page.
Burp will ask if you want to exclude other traffic, or better go to scope settings and add the domain and protocol, that will match any subdomains as well.

## Repeater

In burp, the tab named repeater can be used to tamper with the intercepted http request and send it to the target to see the response.

## Intruder

Example, intercepting a login attempt and capturing that request, right click on it and send to intruder.
In intruder tab, you can add/remove/clear marked text in a request that can be used as a vector for payload.
Let's say there is and id='something' line in the request we can mark that id value with special character to mark it as a attack point and tamper with it's value.
You also need to specify attack type and make a payload, which is going to be different on use case basis.

sudo apt install seclists to get many wordlists in default /usr/share/seclists
ffuf -request file -request-proto http -w /usr/share/seclists/Passwords

ffuf -request login2.txt -request-proto http -mode clusterbomb -w /usr/share/wordlists/wfuzz/others/names.txt:USERFUZZ -w /home/kali/toppasswords.txt:PASSFUZZ
