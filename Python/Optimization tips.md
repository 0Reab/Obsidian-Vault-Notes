```python
nums = [1,1,2,3,4]

return not len(set(nums)) == len(nums)

return len(set(nums)) != len(nums)
```

In the above code snippet, there are two ways to evaluate the same outcome.
Both statements return the same "answer".
But in testing the != inequality line has much faster time complexity.

This is because of the way python is optimized and having "not" as an additional operation in every check adds up quite fast.
The algorithm will perform with O(n) in both cases but that "not" operator adds small overhead every time this is executed.

The rest of the code everything has O(1) time complexity except creating a set out of the list, that is the O(n).
So you can assume that just the "not" part has a somewhat significant chunk of the operations this code executes.