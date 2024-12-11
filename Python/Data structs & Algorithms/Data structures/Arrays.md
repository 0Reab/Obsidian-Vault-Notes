![[Pasted image 20241206143838.png]]

Under the hood index based element referencing is done by accessing the memory address and doing some arithmetic to calculate the address using the index to get the corresponding value at that index.
Like in the diagram above.

The time complexity of this is O(1).
Why, well because we need to compute memory address + index number to get the wanted value.
Which will be the same no matter the size of array.

So how about adding values to a specific list.
```python
array.insert(1, 333) # 1 idx, 333 integer
```
Not to be confused, this method will shift every other element to it's new appropriate index.
Whereas this will be replacing the value at that idx.
```python
array[1] = 333
```

Delete elements
```python
array.pop(1) # idx 1
```


In python array is implemented as dynamic array.
Static arrays have a static size, which makes sense for a low level programming lang.
What would happen is you would get an out of bounds error if you tried to add an element on idx position (len(arr)+1).

