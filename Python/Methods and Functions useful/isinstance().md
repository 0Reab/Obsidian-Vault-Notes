Say this is what you intend to evaluate:
```python
if type(x) is int:
	print(f'{x} is integer')
```

other way of evaluating is.
```python
if isinstance(x, int):
	print(f'{x} is integer')
```

if you'd like to compare x against more than one data types you can pass tuple of data types to check against.
```python
if isinstance(x, (int, float, list, dict, str, tuple)):
	print("x is one of those, True)
```

You can also check if variable is an instance of a class.
```python
class MyObj:
	name = "John"

y = MyObj()

if isinstance(y, MyObj):
	print(f'True, cuz y is an instance of class MyObj.')
```