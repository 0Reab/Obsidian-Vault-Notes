```python
for _ in range(runs): # Good
while runs > 0: # Bad

# _ is a placeholder syntactically, so the code can just run the for loop without assigning anything to a var
```

```python
if num % 2: # Good
if num % 2 == 0: # Bad

# The result of the expression is 0 which is True in python and == 0 is redundant
```

```python
x = [[1, 2], [3, 4]]
return sum(x, []) # [1, 2, 3, 4]

# Sum function in this context concatonates lists together, so the 2D array becomes flatened into 1D arr
```

```python
array = ["apple", "banana", "pear", "grape"]
def sort_by_last(array):
	return sorted(array, key=lambda x: x[-1])
# use key argument for sorting by last char
```

```python
def rotate_array(array, pos):  
    return [array[idx - pos] for idx, _ in enumerate(array)] # Bad  
	return array[-pos:] + array[:-pos] # Good  
  
print(rotate_array([1, 2, 3, 4, 5], 2))  
# Output: [4, 5, 1, 2, 3]
# Simply use indexing ranges from -pos to the end
```

```python
if i == 0 or char != array[i - 1]: # Good
if array[idx + 1] ... # Bad
# First check if idx is 0, if it is the if statmenet goes to execute the if block of itself.
# In case i == 0 evals to false and we check if character is not the same as previous char.
# No need for try except for index out of boud with idx+1 method.
# (in this first if block we append value to result...)
```