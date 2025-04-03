For some problems where we need to process two lists for example.
And have an if statement to process both lists and the same time by using two pointers.
Why this is important is if we decide in that if statement that one or the other item fits the criteria the other ones pointer will still stay on that place for the next loop iteration.

if this description was confusing, lets make an example that's easy to understand.

Task:
Given two sorted (ascending order) lists of integers, combine the two lists while maintaining the sorted order.
```python
list_x = [1,2,3,4]
list_y = [1,3,3,5]

# target = [1,1,2,3,3,3,4,5]

result = [] # stack

x, y = 0, 0 # pointers

while x < len(lst_x) or y < len(y):
	# check which int is smaller
	
	if lst_x[x] < lst_y[y]:
		result.append(lst_x[x])
		# increment pointer
		x += 1 
	else:
		result.append(lst_y[y])
		# increment pointer
		y += 1

# after either of the two pointers (x,y) exceede the length of the list
# take the rest of the lists and add to the end of the result array.

result.extend(list_x[x:])
result.extend(list_y[y:])

# alternatively
result += list_x[x:]
result += list_y[y:]
# using append in this scenario would be an issue like appending an empty list as an item...
```

Hopefully this code snippet illustrates the power of the two pointer method.
each pointer is it's own entity and increments only when needed not when the loop iterates.