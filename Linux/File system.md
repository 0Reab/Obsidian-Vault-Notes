![[Pasted image 20250206181256.png]]

This is the linux file system.
It all starts from the "root" / not to be confused with the path /root.

- /var - is for variable data, aka changing data /var/log is one important path for logs!
- /home - contains home directories of the users on this system, projects, personal files.
- /run - running?
- /etc - shorthand for etcetera, contains configuration files for our network setup, ssh and more.
- /bin - for binaries and executables such as ls, cd, pwd and mount.
- /usr - does not mean user, it's an acronym for universal system resources, so things that you install via package manager will end up here.
- /sbin - system binaries - used only by root for managing the system.
- /proc - virtual file system created when the sys boots and dissolved when sys shuts down - represent current state of the kernel
- /lib - shared libraries and essential kernel modules
- /sys - get information about the system and its components - attached and installed hardware via a virtual file system - cool
- /root - binaries reserved for system maintenance by root user.