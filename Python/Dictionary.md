Consists of key, value pairs.
```python
customer = {
    "name": "John Smith",
    "age": 30,
    "is_verified": True
}
```
How to get a value (2 methods).
```python
customer["name"]
# or
customer.get("name")

# Setting a default value as fallback if .get returns None
customer.get("name", "Fallback name value")
```
Keep in mind calling a key will return it's corresponding value.

Defining:
```python
customer["name"] = "Jack Smith"
```

Getting values, keys from dictionary and deleting.
```python
customer.values()
customer.keys()

del customer[age]
```

```python
fruits = [("Apple", 0.5), ("Banana", 0.3), ("Orange", 0.7)]  
  
dicc = dict(fruits)

# now dicc is a dictionary with k, v pairs of fruit and price.
```