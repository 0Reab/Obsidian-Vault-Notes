In this project I learned how to make my scripts run multiple processes in parallel using xargs command.
Which I previously thought wouldn't be used often due to inability have complex operations within one iteration.
That is not the case if you pass in a function to a bash which is taking in arguments from xargs.

Additionally Terminating a program that has running subshells in parallel has been a challenge until I used signals in bash and kill command.
In order to pass an function and other variables to bash one needs to export the first.

Following will be my research on how these new concepts work with some basic examples and usages.
And how I implemented them in my SSH brute force script.

# How to use xargs

Xargs can be used as a replacement of a while loop that reads in text line by line.
***How is xargs different than pipe?*** 
Xargs differs from pipe because piping takes output of one command and sends it as an input to another command, while xargs takes the output of the command but it sends it as an argument to the other command as the name suggests.

Example while loop for reading txt:
```bash
while read -r line; do
	echo "$line"
done < "$txt_file"
```
As you can see three lines of code with input redirection can be achieved with xargs:
```bash
cat "$txt_file" | xargs -I {} echo {}
```
Where in this instance every {} will be replaced with argument.

# Trap 
Trap is a command that is used to trap signals that occur while your script runs.
Signals can be from system or user (ctrl+c for example).

```bash
function interrupt() {
	echo "Stopping the program"
	exit 0
}

trap interrupt SIGINT # signal interrupt
```

# Bash -c
This command spawns a non-interactive shell and it inherits the environment of it's original shell just like subshell.
It can execute the string that is passed to it that is detached from your current terminal and you can pass arguments to it.

```bash
cat "$wordlist" | xargs -P 2 -I {} bash -c 'my_function "@"' _ {}
```
In this example we combine the xargs as mentioned before and bash -c.
What happens here is bash -c spawns twice as stated in -P 2 in xargs.
Effectively achieving parallel execution.

Whatever code is in my_function called with all arguments ("@") passed into it.
Will run with arguments that xargs sends at the {} position.

The underscore before _ the {} is an empty placeholder denoting nothing is passed as argument $0, so the next argument at {} is $1 which will end up in my_function, it comes form xargs which pulls passwords from cat command.

