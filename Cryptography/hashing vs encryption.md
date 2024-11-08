**Hashing vs. Encryption:** The difference between hashing and encryption is that hashing algorithms are non-reversible (one-way functions), whereas encryption algorithms are reversible (two-way functions).

**Hashing Algorithms:** Computing hashes using different algorithms such as the outdated MD5 or secure algorithms like SHA-256 will compute a hash of a given string. Hashing algorithms will always produce a string of a fixed length specific to the algorithm used (e.g., MD5 produces a 128-bit hash, SHA-256 produces a 256-bit hash).

**Salting Hashes:** A hashed password can be salted by adding an additional value (salt) to compute a different hash for a given string. In other words, if I were to compute the hash for the string 'password' and you did the same, we would get the same hash. However, if you added salt to your hash, your computed hash value would be different. Salting helps to protect against precomputed hash attacks, such as rainbow tables, by ensuring that even identical passwords result in unique hashes.

**Dictionary Attacks:** Dictionary attacks are brute force attacks that could crack weak passwords by systematically testing all possible passwords from a pre-defined list (dictionary) of likely passwords, which often includes common words, short passwords, and those without numbers or special characters.

**Rainbow Attacks:** A rainbow attack involves using a precomputed table of hashes for many possible strings, which can take up a significant amount of storage. These tables allow attackers to quickly look up the hash of a password to find its plaintext equivalent, but are mitigated by salting hashes.

Hashing functions that are 128-bit for example produce 128-bit hashes, which is basically length of the string, those hashes are computed in binary and converted in base16 or hex for readability. Higher bit number = better for security and it exponentionaly reduces the chance of collision meaning two different values producing the same hash.