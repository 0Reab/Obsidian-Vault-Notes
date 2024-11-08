Here is an basic if statement in bash.
```bash
if [ 5 -lt 3 ]; then
	echo "5 is less than three."
else
	echo "This will not execute."
fi
```

And here is a fancy if statement.

```bash
if [ 5 -lt 3 ] && echo "5 is less than three." || echo "This will not execute."
```

Using &&, || (AND, OR [[Bash/List of Useful features/Logic gates]] respectively) we can accomplish if else logic.