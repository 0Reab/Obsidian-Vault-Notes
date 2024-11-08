Command "eval" is used to evaluate a string as a command and run it.

```bash
cmd="sudo -l"

eval "$cmd"
```

This code snippet evaluates the string assigned to the variable cmd and runs it.

Very insecure, especially if there is bad input validation or safety checks in place that would prevent this from running some rouge string and evaluating it.