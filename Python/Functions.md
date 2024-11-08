Making a function that takes in arbitrary number of arguments.
```python
def add(*args):
	result = 0
	
	for i in args:
		result += i
		
	return result
# the keyword args can be named anything, just like
# for i in list, but for best practices args is very commonly used
```

Unpacking arguments
Let's say we have a function that takes in x number of arguments.
And the function calculates something and returns a value.
For the arguments, we gather the data to use in the calculation, and in case we have lots of arguments to pass in we can use unpacking to make our code cleaner.

```python
def calculate_score(points, mvps, deaths, assists):
	return points - deaths + (assists * 0.5 + mvps * 2)

points = 11
mvps = 3
deaths = 5
assists = 4

stats = [points, mvps, deaths, assists]

calculate_score(*stats) # unpacking the list stats as arguments for a function call
```