```shell
ports:
	22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
	80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))

paths:
	/admin                (Status: 301) [Size: 310] [--> http://10.10.77.86/admin/]
	/etc                  (Status: 301) [Size: 308] [--> http://10.10.77.86/etc/]

users:
	Alex:S3cretP@s3
	Adam
	Josh

passwd = music_archive:$apr1$BpZ.Q.1m$F0qqPwHSOG50URuOVQTTn. - cracked lol --> squidward
hashes from final_archive cracked to !!!josh14!!!.a6_123

This is a Borg Backup repository.
See https://borgbackup.readthedocs.io/

finds:
	summerized:
		algorithms - sha256 XXH64
	[repository]
	version = 1
	segments_per_dir = 1000
	max_segment_size = 524288000
	append_only = 0
	storage_quota = 0
	additional_free_space = 0
	id = ebb1973fa0114d4ff34180d1e116c913d73ad1968bf375babd0259f74b848d31
	key = hqlhbGdvcml0aG2mc2hhMjU2pGRhdGHaAZ6ZS3pOjzX7NiYkZMTEyECo+6f9mTsiO9ZWFV
	        L/2KvB2UL9wHUa9nVV55aAMhyYRarsQWQZwjqhT0MedUEGWP+FQXlFJiCpm4n3myNgHWKj
	        2/y/khvv50yC3gFIdgoEXY5RxVCXhZBtROCwthh6sc3m4Z6VsebTxY6xYOIp582HrINXzN
	        8NZWZ0cQZCFxwkT1AOENIljk/8gryggZl6HaNq+kPxjP8Muz/hm39ZQgkO0Dc7D3YVwLhX
	        daw9tQWil480pG5d6PHiL1yGdRn8+KUca82qhutWmoW1nyupSJxPDnSFY+/4u5UaoenPgx
	        oDLeJ7BBxUVsP1t25NUxMWCfmFakNlmLlYVUVwE+60y84QUmG+ufo5arj+JhMYptMK2lyN
	        eyUMQWcKX0fqUjC+m1qncyOs98q5VmTeUwYU6A7swuegzMxl9iqZ1YpRtNhuS4A5z9H0mb
	        T8puAPzLDC1G33npkBeIFYIrzwDBgXvCUqRHY6+PCxlngzz/QZyVvRMvQjp4KC0Focrkwl
	        vi3rft2Mh/m7mUdmEejnKc5vRNCkaGFzaNoAICDoAxLOsEXy6xetV9yq+BzKRersnWC16h
	        SuQq4smlLgqml0ZXJhdGlvbnPOAAGGoKRzYWx02gAgzFQioCyKKfXqR5j3WKqwp+RM0Zld
	        UCH8bjZLfc1GFsundmVyc2lvbgE=
	
	
	�version^B�hints�^@@{"algorithm": "XXH64", "digests": {"final": "05178884e81563d7"}}�index�^@b{"a>
	
	nonce = 0000000200000b9
	his is a Borg Backup repository.
	See https://borgbackup.readthedocs.io/
	
	algo = sha256 data
	
	BORG_SEG@
	BORG_IDXd
	
	�version^B�hints�^@@{"algorithm": "XXH64", "digests": {"final": "05178884e81563d7"}}�index�^@b{"algorithm": "XXH64", "digests": {"HashHeader": "146e9cb969e480a3", "final": "b53737af67235823"}}

What i learned:
	$apr1 is not just a salt value, it was also an indication about the has type/id itself, use hashcat website / formats to research
	but its ok, cracked the hash anyway with hash-identifier no probs.
	** important
	Kinda tricky to realize there is an archive that is hiding in the files that you download.
	But reading borg docs and having the borg installed in the first place gets you started.
	using some basic usage with borg like list or extract and using the cracked psws to read and extract the archive.
	So many files made it hard to know where to look, but step by step slowly think and no problem.
	Other than that priv esc was interesting, using history command revealed how to inject commands into the only passwordless sudo thing you can run.
	But on the other hand if you can read bash, it's kinda easy to spot regarding the get opts part and how that is executed, the -c part for option wasnt obvious tho?

```