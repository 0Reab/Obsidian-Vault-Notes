Is a short function that is used in one off usage cases.

```python
x = lambda num: num+10
y = 5

print(x(y))
# prints 15 because 5 as argument in lambda 5+10
```

If statements in lambda
```python
over_limit = lambda spent: True if max(spent) > 2000 else False
```