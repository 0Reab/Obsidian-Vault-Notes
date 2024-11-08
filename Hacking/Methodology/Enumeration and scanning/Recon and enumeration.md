1. Tech profiling: 
	1. builtwith.com - Website.
	2. wappalyzer - Browser extension.
	3. securityheaders.com - Website
	4. curl -I -L "link" - Grab header, follow redirects.
	5. nmap -p"portnum" -A target.com - Fingerprinting.
2. Directory enumeration:
	1. ffuf -w wlistpath.txt:FUZZ -u "link"/FUZZ
	2. Dirbuster - kali tool
	3. dirb - kali tool
3. Subdomain enumeration:
	1. Example dev.site.com, subdomain = 'dev'
	2. site:site.com -www -store: google dorking exclude with minus (filetype:pdf etc)
	3. crt.sh - Website. (% for wildcard for subdomain)
	4. subfinder -d site.com - (-d = domain) tool
	5. assetfinder - tool, enum -d 
	6. amass - tool, 
	7. httprobe - tool, pipe found subdomains to this command to check if they are live.
	8. gowitness - tool, file -f alaviesubdomains.txt -P ~/screenshotsofsites --no-http 