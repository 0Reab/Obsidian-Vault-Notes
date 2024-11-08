Functions can have local variables in them.
Those variables are going to be accessible only inside the function which we can call the scope of the variable.
In other words calling this variable that is local to a specific function will not be successful.

```bash

function greetings() {
	local name="$1" 
	# define local variable name as the first argument upon a function call
	
	echo "Hello $name"
}

greetings "Harry" 
# call the fucntion with first argument string "Harry" (aka $1)

```

Functions in C [[C/Functions]].
Functions in python [[Python/Python function|Python function]]
