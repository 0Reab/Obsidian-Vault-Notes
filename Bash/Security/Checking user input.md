In any script it's important to validate and check any input that is required from the user.
Because mistakes can happen either intentional or not, and they can cause unwanted behavior of your scripts.

For example checking if the input is a file or using a regex to validate an ipv4 address...
```bash
read -p "Enter a filename." file
read -p "Enter a directory path." path

if [ ! -f "$file" ]; then # check if it's a file
	echo "$file is not a file."
	exit 1
fi

if [ ! -d "$path" ]; then # check if it's a path
	echo "$path is invalid."
	exit
fi
# otherwise continue the execution
```
Example of [[Validating IPv4 with regex]].

Reason being for all of this effort is if that input string were to be evaluated by a command such as netcat or even worse [[Eval]] command.

Here is an example script that does firewall auditing and is expecting ports as arguments to parse and perform checks on.
It has no safety checks for the cli arguments.

More on CLI arguments and more.
[[Parsing switches]].
[[Arguments]].
[[Reading user input one liner]].

```bash
# add later
```

