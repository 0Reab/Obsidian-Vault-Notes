Often when looping over an array for example and you would like to keep track of the index.
At first go you might do something similar:
```python
array = [1,2,3,4]
count = 0

for num in array:
	print(num)
	count += 1
```

Instead using enumerate you can do it better.
```python
array = [1,2,3,4]

for idx, num in enumerate(array):
	print(num)
	print(idx)
```