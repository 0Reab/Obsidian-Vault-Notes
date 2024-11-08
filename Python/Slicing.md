Python slicing with indexes can be done with positive or negative integers.
```python
[-3, -2 ,-1 ,0 ,1 ,2 ,3 ]
```
Using : symbol to denote in which direction (positive/negative) should the indexing continue.

```python
string = "Example"

print(string[start:end:step]) # explanation of slicing

print(string[0:]) # form first to last character
print(string[:-1]) # from last to first character
```

```python
print(string[0]) # first character, 1 for second char
print(string[-1]) # last character, or -2 second for last char
```

Example:
```python
string = "Example"

print(string[1:6:2]) # start 1, end 6, step 2 for slicing

# keep in mind end is non inclusive meaning we need one more index over 
# aka to get index 5 your end should be index 6 or above.

output: "xml"
# Example
#  1 3 5

# alternative using negative indexing for same result
print(string[-6:-1:2])
```
