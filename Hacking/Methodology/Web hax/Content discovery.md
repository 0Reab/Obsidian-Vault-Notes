This refers to discovering content that was not intended for the public.
That content can be anything from files such as images, text documents or old website backups or databases.

There are three methods of content discovery.

- Manual
- Automated
- OSINT (open source intelligence)

## Manual

#### Robots.txt

This text file exists for web crawlers and spiders to read and know what /directories are allowed for crawling and which are not.
It also tells search engines what pages not to display in searches.

#### Favicon

This is a tiny icon that shows up in your tabs of the websites you visit.
Sometimes If the default favicon is not replaced with a custom one.
It can be used to learn about what framework is in use.

You can use this command to download the favicon.
```shell
curl https://static-labs.tryhackme.cloud/sites/favicon/images/favicon.ico | md5sum
```
At the end the md5sum is calculated and you can use this favicon data base to find out it's origin.
https://wiki.owasp.org/index.php/OWASP_favicon_database

```powershell
# Powershell alternative
curl https://static-labs.tryhackme.cloud/sites/favicon/images/favicon.ico -UseBasicParsing -o favicon.ico

Get-FileHash .\favicon.ico -Algorithm MD5
```

#### Sitemap.xml

This is similar to robots.txt but this one serves the purpose to tell search engines what they would like to be indexed.
Sometimes it can include directories that work behind the scenes but are not easily accessible.

#### HTTP headers

Can contain useful information about the technology and systems that are utilized to make this web app run.
That can be PHP version and server type and version.

#### Framework stack

If we manage to find out the CMS / framework stack, we can use this information to look up the documentation of the mentioned framework.
This can be useful for looking at admin portal (login form for admin panel) and more.

Additionally, knowing the framework can help you with directory enumeration with framework specific wordlists.

## OSINT

Referring to open source intelligence as in information.
Can be used to find employee emails and naming conventions.
Documents that aren't meant for public eyes.

#### Google dorking

These keywords can be used to customize your search and get more specific results.

|            |                    |                                                              |
| ---------- | ------------------ | ------------------------------------------------------------ |
| **Filter** | **Example**        | **Description**                                              |
| site       | site:tryhackme.com | returns results only from the specified website address      |
| inurl      | inurl:admin        | returns results that have the specified word in the URL      |
| filetype   | filetype:pdf       | returns results which are a particular file extension        |
| intitle    | intitle:admin      | returns results that contain the specified word in the title |

#### Wappalyzer

Is an online tool that can find CMS and framework of and web app, payment processors and much more (possibly and version number).

#### Wayback machine

Is an online archive of the internet.
It can help you with information gathering and maybe you will find old pages that are still active on the current website.
https://archive.org/web/

#### Github

Is an code repository, and as such you may find source code, passwords and god knows what else.

#### S3 Buckets

Is a storage service offered by Amazon AWS, and they can be publicly accessible.
The url ends in .s3.amazonaws.com.

## Automated discovery

Is when you use tools that automatically discover stuff, utilizing wordlists.
Each has it's pros and cons, but here are few.
ffuf (fuzz faster u fool), gobuster, dirb.

**Using ffuf:**

```shell-session
user@machine$ ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -u http://10.10.101.72/FUZZ
```

**Using dirb:**

```shell-session
user@machine$ dirb http://10.10.101.72/ /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
```

**Using Gobuster:**

```shell-session
user@machine$ gobuster dir --url http://10.10.101.72/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
```

