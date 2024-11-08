```python
a = min([1,2,3])
b = max([1,2,3])

print(a, b)
# output :
1
3
```

These functions take in argument key=...
The key tells the function what is going to be processed in case elements of the array happen to be tuples.
You could tell the function via key argument to do it's functionality on a specific index of said tuple.

For example if you had:
```python
fruits = [ ("Apple", 0.4), ("Orange", 0.3) ]

minimum = min(fruits, key=lambda item: item[1] )
```

To access the price of each tuple which is an iterable of our list fruits.
As it's passed in to the min function, to be able to iterate over the prices we need to index them via the lambda function for example like here via item at index 1.

[[Lambda]]