```python
# Write a program which can map() and filter() to make a list whose elements are square of even number in [1,2,3,4,5,6,7,8,9,10].  
  
numbers = [1,2,3,4,5,6,7,8,9,10]  
  
even_nums = list(filter(lambda x: x%2 == 0, map(lambda z: z**2, numbers)))  
  
print(even_nums)
```