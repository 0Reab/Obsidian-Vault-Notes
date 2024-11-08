This is a basic print statement:
```python
print("Hello world!")
```

How to print variables value?
```python
print("Hello " + name + ", I hope you are well!") # this is called concatenation
```

Alternatively, with fstrings:
```python
printf("Hello {name}, I hope you are well!")
```

Raw string, python will not try to interpret escape characters and else in the string.
```python
print(r'C:\Users\Mihajlo\new_folder') # \n is an escape char for new line
```