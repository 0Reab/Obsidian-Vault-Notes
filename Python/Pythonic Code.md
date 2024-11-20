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