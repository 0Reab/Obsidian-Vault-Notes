```shell
notes:
	Don't brute force any login pages, just need a browser and remote desktop.
	There are 4 FLAGS.	
	the machine doesnt reply to PINGS, ffs just run nmap

to do:
	ok - Look at robots.txt - task says, waht is a possible psw where crawlers look. - UmbracoIsTheBest!
	Find out CMS of the website -
	What is the domain of the website -
	What is the name of Administrator -
	What is the email address of the administrator -
	steganography on downloaded images.	
	Nikto -h scan -
	fuzz sql injections on cms login and maby search box

paths:
	/search               (Status: 200) [Size: 3418]
	/blog                 (Status: 200) [Size: 5394]
	/sitemap              (Status: 200) [Size: 1041]
	/rss                  (Status: 200) [Size: 1873]
	/archive              (Status: 301) [Size: 123] [--> /blog/]
	/categories           (Status: 200) [Size: 3541]
	/authors              (Status: 200) [Size: 4115]
	/Search               (Status: 200) [Size: 3468]
	/tags                 (Status: 200) [Size: 3594]
	/install              (Status: 302) [Size: 126] [--> /umbraco/]
	/RSS                  (Status: 200) [Size: 1873]
	/Blog                 (Status: 200) [Size: 5394]
	/Archive              (Status: 301) [Size: 123] [--> /blog/]
	/SiteMap              (Status: 200) [Size: 1041]
	/siteMap              (Status: 200) [Size: 1041]
	/INSTALL              (Status: 302) [Size: 126] [--> /umbraco/]
	/Sitemap              (Status: 200) [Size: 1041]
	/1073                 (Status: 200) [Size: 5394]
	/Rss                  (Status: 200) [Size: 1863]
	/Categories           (Status: 200) [Size: 3491]
	/1074                 (Status: 301) [Size: 118] [--> /]
	/*checkout*           (Status: 400) [Size: 3420]
	/1078                 (Status: 200) [Size: 6155]
	/Authors              (Status: 200) [Size: 4070]
	/1075                 (Status: 200) [Size: 4070]
	/1079                 (Status: 200) [Size: 6215]
	/1076                 (Status: 200) [Size: 5021]
	
robots.txt:
	# Define the directories not to crawl
	Disallow: /bin/
	Disallow: /config/
	Disallow: /umbraco/
	Disallow: /umbraco_client/

finds:
	source-code: flag 2 - THM{G!T_G00D}
	all rest flags are in source code 'cross teh pages
	source-code:  http://10.10.19.175/media/articulate/default/capture3.png
	some image: http://10.10.19.175/media/articulate/default/random-mask.jpg
	on page   : flage 3 - THM{L0L_WH0_D15}
	author txt: James Orchard Halliwell --> 
	hiring per: JD@anthem.com Jane Doe
	website exploded: http://10.10.19.175/search?term=%27+*+from+
	CMS : Umbraco
	CMS login : http://10.10.19.175/umbraco/#/login
	+ /: Web Server returns a valid response with junk HTTP methods which may cause false positives.
	Microsoft-IIS/10.0
	f-in ctf moment: the poem is meant to be looked up, cuz admin wrote it, and find the name: solomon grundy
	cms login success - sg@anthem.com:UmbracoIsTheBest! (admin ezpez)
	└─$ xfreerdp /u:sg /p:UmbracoIsTheBest! /v:10.10.19.175 this is how you rdp login, I tried on first name basis, didn't work.

win privesc: search windows 19 server 10 priv
	OS Name: Microsoft Windows Server 2019 Standard
	OS Version: 10.0.17763 N/A Build 17763

	 TA0004 - Privilege Escalation
	 - Latest updates installed → Medium
	 TA0006 - Credential Access
	 - LSA Protection → Low
	 - Credential Guard → Low

	 TA0003 - Persistence
	 - UEFI & Secure Boot → Low
	 - COM server missing module files → Low
	 TA0004 - Privilege Escalation
	 - Latest updates installed → Medium
	 TA0006 - Credential Access
	 - LSA Protection → Low
	 - Credential Guard → Low

users:
	solomon grundy sg@anthem.com (admin)
	james orchard halliwell joh@anthem.com (idk bout email)
	jane doe jd@anthem.com (hiring person)
	

What I learned:
	If you know the CMS, consider using wordlist specific to that CMS, you might discover lots more.
	read source code dumb fuck	


```