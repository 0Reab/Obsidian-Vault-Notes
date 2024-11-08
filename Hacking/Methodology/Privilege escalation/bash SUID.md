If you can run a shell command with higher privileges.
One thing you can do instead of running a whole new reverse shell, is to set SUID bit in binary /bin/bash

What this does is if the /bin/bash is owned by a root for example the SUID bit will allow this binary to be executed with privileges of the owner (root in this case).

```sh
chmod +x /bin/bash
/bin/bash -p
```