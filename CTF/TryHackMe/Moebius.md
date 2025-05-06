php
image lookup url parameters 
-> 
hash= 
path=/var/www/images/cat13.jpg
short_tag=cute -> ** SQL injection error wrong tag ** -> MariaDB -> SQLSTATE[42000]
back-end DBMS: MySQL >= 5.0 (MariaDB fork) - released 2005 - vulnerable?
Syntax error or access violation: 1064
;-- invokes injection protection
->

qr code img -> i got fucking rick rolled
http://10.10.77.178/image.php?hash=953d2306cb51044b11b7916d7a81cea0a49b22e457d5708b7f4f0561c4fc2695&path=/var/www/images/cat7.png

ideas:
check mariadb ports
check requests - network traffic -> x
path traversal?
if hash= is empty - php error? -> Warning: Undefined array key "path" in /var/www/html/image.php on line 15
Image not found
maybe RCE possible

22/tcp open  ssh     OpenSSH 8.9p1 (protocol 2.0)

PHP/8.4.4 - latest -> 8.4.6 -> change log url -> https://www.php.net/ChangeLog-8.php#8.4.4
Apache/2.4.62 (Debian)

GET parameter 'short_tag' is vulnerable

gobuster medium list - nothing


 Connection failed: SQLSTATE[42S22]: Column not found: 1054 Unknown column 'qqjpqshhuWfylrHGZnKtoJgvWUOEvNdQonQtszvADPtYnqzppq' in 'WHERE' 

`http://10.10.235.226/album.php?short_tag=-6479%27%20UNION%20ALL%20SELECT%20CONCAT(0x71716a7071,0x736868755766796c7248475a6e4b746f4a677657554f45764e64516f6e5174737a7641445074596e,0x717a707071)--%20-`


 Connection failed: SQLSTATE[42S02]: Base table or view not found: 1109 Unknown table 'PL' in information_schema 

`http://10.10.235.226/album.php?short_tag=smart%27%20AND%20(SELECT%209029%20FROM(SELECT%20COUNT(*),CONCAT(0x71716a7071,(SELECT%20(ELT(9029=9029,1))),0x717a707071,FLOOR(RAND(0)*2))x%20FROM%20INFORMATION_SCHEMA.PL%20%20%20%20UGINS%20GROUP%20BY%20x)a)%20AND%20%27fWvH%27=%27fWvH`

ssh user -> web ?


Lessons learned:
I got stuck after all of this recon.
What I've manage to gather is, that sql injections are possible, ssh is rate limited and open, php url indicated some tampering could be done with it.
And possibly judging by the images, maybe utilizing all of those things to leverage a shell in combination possibly write and leak data via SQL and run the php shell by trying to access the file from the web page.
But lacking in skill and knowledge to proceed.

Reading write up and learning:

First thing I notice, the person runs
`nmap -p- <ip_addr>` and then
`nmap -sC -sV -p22,80 <ip_addr>`
My understanding is we first quickly go thru whole port range and just check if ports are open, and then use the open ports to gather more info such as version detection etc.
This saves time instead of testing with those scripts on each and every port when most of them won't need that, not too sure.

After exploring the web app (image album) each album when open in source html shows ID in comments, the hint i missed.
The url path for these albums and images are `/image.php?hash=953d2306cb51044b11b7916d7a81cea0a49b22e457d5708b7f4f0561c4fc2695&path=/var/www/images/cat7.png`
So we got a hash and path parameters for a file.
For the future it might be a good idea to identify the algorithm of the hash, by using the go' old hash identifier, a python tool.

In my opinion i think it was a good idea to check path traversal on the path parameter, while having hash value as something that will just have the app not throw an error.
By some intuition my guess was that the hash function was generated from the file with something like checksum almost, or a combination of a file and the path.
sqlmap later showed that the hash parameter was not injectable.

Very happy that I tested some basic sql tests manually on `short_tag=` parameter and got and sql error, and eventually tried `;` in injection which triggered some sort of a defense.
That tag param ended up being vulnerable to three types of sql injection variants bool, error, blind time based injections.
I did dump the database as the person in writeup did, but I think for the future use `--dump` instead of `--dump-all` because it took super long and I think I dumped 90% of the things that were useless to me.

It seems like using `/` in the short_tag param also caused defense to trigger, which makes it harder to do LFI (local file inclusion - leaking files or running files).
The defense also stops execution of stacked queries such as `insert into` for LFI, (stacked queries are just complete sql statements).

The `/` filter can be bypassed via hex encoding, that never came to my mind, just a basic url encode was my idea that failed.

After all of this is said, author mentions that this might solely be about LFI vuln that leads to RCE of the image.php page.
When accessing the album.php?short_tag=smart all entries are loaded and hash value is generated hence the hashes are not stored in the DB.

The idea is to insert a path in the images table to read arbitrary files.
and see if we can read or write files via sql even tho ';' is filtered and stopping stacked queries.

Via sqlmap we intercept error based sql injections `--dump --proxy`.
The reason why we use error based is because it leaks data with errors directly in response.



