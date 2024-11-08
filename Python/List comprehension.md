One of the coolest python features is list comprehension.
[[Data types list]]

Here is an example of multiplying each item by 2.
```python
nums = [ 1, 2, 3 ]

[ i * 2 for i in nums ]

output:
[ 2, 4, 6 ]
```

Making a list with just one out of two values passed in as iterable.
```python
fruits = [("Apple", 0.5), ("Banana", 0.3), ("Orange", 0.7)]

summed = sum([ price for _, price in fruits ])
# the _ is a place holder that just ignores whatever is the [0]-th value and assigns value at [1] as price.
# Similarly to for k, v in dict the _ just replaces an iterable variable if you don't need it, like in our case summing the prices ergo we don't need names.
```

In this example we form a new list based on a condition for each item in first list.
```python
list=[1,2,3]

new_list=[ item for item in list if item == 3 ]

output:
[3]
```

```python
a = [ 1, 2, 3 ]

result = list(map(lambda i: i * 5 if i == 3 else i, filter(lambda i: i > 1, a)))
result2 = [ i * 5 if i == 3 else i for i in a if i > 1 ]

result3 = []
for num in a: # same thing but in a basic for loop form
    if num > 1:
        if num == 3:
            result3.append(num*5)
        else:
            result3.append(num)

output: # These are equivalent

[2, 15]
[2, 15]
[2, 15]
```
Multiple if statements:
```python
nums = [ 1, 2, 3 ]

for i in nums:
    if i > 1:
        if i != 3:
            print(i)

# is equivalent to

[ i for i in nums if i > i if i != 3]

for i in nums:
    if i > 1 and i != 3:
        print(i)

# is equivalent to

[ i for i in nums if i > i and i != 3]
```
Looping over two lists
```python
nums = [ 1, 2, 3, 4 ]
fruit = [ 'Apples', 'Peaches', 'Pears', 'Bananas' ]

result = [ print(i, f) for i in nums if i > 1 for f in fruit ]
```