In bash *if* statements are used to execute a block of code *if* a condition is met.
```bash
if [ -e textfile.txt ]; then
	echo "text file exists"
else
	echo "text file does not exist"
fi
```
In this example we are checking if a file textfile.txt exists using -e.
Alternatively you can check for directories using -d or for files -f.
-e will check for directory and files.

Additionally if you need multiple if statements you can use elif (else if) and don't forget to ad fi at the end!

Combine this with [[Bash/Variables/logic gates|logic gates]]. (and for C [[C/Logic gates|Logic gates]])

If statements in C [[If statements]].
If statements in bash expanded [[Fancy IF statements]].
If statements in python [[Example]], [[Boolean Vaule]]