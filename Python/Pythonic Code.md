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

```python
def digit_sum(array): # decent but ... 
	summed = [ sum([int(i) for i in str(n)]) for n in array ]
	return [ i[1] for i in sorted(zip(summed, array), key=lambda x: x[0]) ]

# check this bad boy out
def digit_sum(array):
	return sorted(array, key=lambda n: sum(int(digit) for digit in str(n)))

# so in order to sort by summed digits of each element
# you don't need to make var summed
# you can make that list of summed digits inside the sorted as a lambda function
# which does the same as last code, just the key for sorting is directly in lambda

# rather than digging thru an tuple for i[1]
```

```python
def flatten(data): # horrbile (using strings big no no)
    data = str(data)
    for c in '[],':
        data = str(data).replace(c,'',len(data))
    return [int(i) for i in list(data) if i != ' ']

def flatten(data):
	result = []
	for item in data:
		if type(item) is list:
			result.extend(flatten(item)) # flatten is recursive func call
			# extend works like lst concat [1,2] and [3,4] = [1,2,3,4]
		else:
			result.append(item)
	return result
```

```python
from collections import Counter
# use this more
# instead of
array.count(x) 
# it is a C implementation built in that is fast, in cases whre O(n **2) is time complexity
# could be reduced to O(n)

from collections import Counter

def finder(array):
	counts = Counter(array)
	
	for num, freq in counts.items(): 
		if freq == 1: 
			return num
```

```python
from collections import Counter  

def most_frequent(array): 
	# can be simpler
    return max([(num, freq) for num, freq in Counter(array).items()], key=lambda x: x[1])[0]  

	# like this
    return max(Counter(array).items(), key=lambda x: x[1])[0]  
	  
#print(most_frequent([1,3,2,3,1,3]))  
# 3
```

```python
from itertools import chain  
  
def matrix_addition(a, b):  
    result = []  
    chain_one, chain_two = list(chain.from_iterable(a)), list(chain.from_iterable(b))  
    length = len(a[0])  
    summed = list(reversed([ num + chain_two[idx] for idx, num in enumerate(chain_one) ]))  
  
    for i in summed:  
        tmp = []  
        for _ in range(length):  
            tmp.append(summed.pop())  
        result.append(tmp)  
  
    return result  
  
def matrix_addition(a, b):  
    return [[sum(xs) for xs in zip(ra, rb)] for ra, rb in zip(a, b)]
```

``` python
over_limit = lambda spent: True if max(spent) > 2000 else False
# lambda if statement example
```