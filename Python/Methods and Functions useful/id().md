The function id serves to identify objects.
Each object has a unique number as it's identifier.

This can be utilized for debugging to help you understand if something is simply a pointer to an object which is not dynamically created.
For example this bug I've created while making a 2d array for a snake game level.

the idea was to assign the snake 1x1 to a specific xy coordinate in the 2d array.
something like this:
```python
array = [
	'*', ' ', ' ',
	' ', ' ', ' '
]
```
here we assign the snake to `array[0][0]`.

But what I got instead was the row being fully filled from top to bottom with snake.

the reason why was in what way I constructed the array aka level.
```python
row = [ ' ' for _ in range(3) ]
array = [ row for _ in range(2) ]
```
In this instance when we reference row in the array list comprehension we are simply pointing to the same object each iteration.
thus debugging with id(i) I realized each row is the same object.

The solution to this was to have nested row list comprehension within my array construction (one line).
In that case rows were not simply a pointer to a list they which when modified once was modified indefinitely.
Rather dynamically generated at execution if you will.

so in a case where `array[0][1]` was meant to be the snake we are affecting the object in the row variable.
Thus modifying an object that is unchangeable in the execution of array variable list comprehension.

```python
size = range(5)  
  
row = [ ' ' for _ in size ]  
array = [ row for _ in size ]  
# array = [ [' ' for _ in size] for _ in size ]  
  
  
array[3][4] = '*'  
  
for i in array:  
    print(f'{i} -> {id(i)}')
```