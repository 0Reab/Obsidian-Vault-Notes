To begin with, let’s first understand what command injection is. Command injection is the abuse of an application's behaviour to execute commands on the operating system, using the same privileges that the application on a device is running with. For example, achieving command injection on a web server running as a user named `joe` will execute commands under this `joe` user - and therefore obtain any permissions that `joe` has.

Command injection is also often known as “Remote Code Execution” (RCE) because of the ability to remotely execute code within an application. These vulnerabilities are often the most lucrative to an attacker because it means that the attacker can directly interact with the vulnerable system. For example, an attacker may read system or user files, data, and things of that nature.  

For example, being able to abuse an application to perform the command `whoami` to list what user account the application is running will be an example of command injection.

Command injection was one of the top ten vulnerabilities reported by Contrast Security’s AppSec intelligence report in 2019. ([Contrast Security AppSec., 2019](https://www.contrastsecurity.com/security-influencers/insights-appsec-intelligence-report)). Moreover, the OWASP framework constantly proposes vulnerabilities of this nature as one of the top ten vulnerabilities of a web application ([OWASP framework](https://owasp.org/www-project-top-ten/)).

#### Examples

If a web application utilizes a shell script to perform some functionality for the app.
And it uses dangerous commands/functions such as `exec()`, `shell()`, `eval()`.

**LAB** - There is an OS command injection in a "submit feedback" form that utilizes shell script, cause a response delay.

**Solution** - Tampering with the requests POST data, email parameter seems to cause error "could not save data" if we add ; or a quote or maybe anything else.
Tip for next time, instead of using %20 as URL encoding to write payloads spaces just use +, works the same.
Alternatively using && as AND operator in shell might work oddly because & is recognized as parameter parameter in HTTP request post data.

What you could've inferred from this error in email param is that after using ; to add command fails.
Is to simply us OR operator `||` and then add desired command after.
So my payload: `X||sleep+10`

Simple sleep command to verify blind RCE by response time hanging for 10s.
The solution example uses ping -c 10 "ipaddr".
Which seems less reliable than sleep to me but still works ok for PoC.


**LAB** - Same like previous but you can utilize output redirection on a writeable `/var/www/images/` so the RCE will not be blind completely. (run whoami to solve).

**Solution** - same like previous lab email parameter is vulnerable to RCE.
Test payload:   `X+||+echo+'rce'+1>/var/www/images/rce.txt`

In the test payload we simply echo the string rce into a text file.
Which we can access by opening an image of the webapp and finding it's source URL and changing the image name to text filename.

Final payload: `X+||+whoami+1>/var/www/images/whoami.png`

The response indicated error which made me think command was failing for some reason but seems like just some other part of the script was responding with error.
Because the path `/image?filename=rce.txt` contained the following:
`sh: 1: peter-L4QSLG: not found`


