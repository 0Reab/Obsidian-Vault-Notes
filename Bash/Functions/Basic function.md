*What are functions and why should you use them?*

Functions are used for making a small code snippet that does something useful and allows you to reuse that code with possibly different values, and you will have the ability to just call the functions name instead of repeating your code.

Expample:
```bash
function add() {
	echo $(( $1 + $2 ))
}

x=1
y=2

#calling the function with x and y as parameters
add $x $y
```

In this example we have a function that can add two variables together via arithmetic expansion.

You may notice inside the function, variables $1 and $2.
They represent a variable that will be assigned a value upon a function call in that order.
Therefore $1 takes the value of $x when we call the function, and $y takes the value of $2.