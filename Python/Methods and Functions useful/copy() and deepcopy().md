We can use methods copy and deep copy in python in order to make a copy of data so we can change it without changing the original.

```python
import copy
# copy also works with slicing [:]
def remove_smallest(numbers):
	a = numbers[:] # alternative method
	a = copy.deepcopy(numbers) # original method but it reqs import copy
	a.remove(min(a))

	return a
```

Now the difference between deep copy and copy (shallow copy).
The shallow copy copies just the new array but it does not create new copies of elements within the array.
Instead it points to the same elements from original array.
Meanwhile deep copy makes the copies the new array and the elements too so everything is independent from original data.

Refer to pointers for deeper understanding.
[[Pointers]]