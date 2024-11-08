```python
stocks = {  
    'gogle': 234,  
    'asdf': 4353,  
    'vdf': 546,  
    'f': 23  
}  
  
test = zip(stocks.values(), stocks.keys())  
  
print(list(test))  
  
# output  
# [ (234, 'gogle'), (4353, 'asdf'), (546, 'vdf'), (23, 'f') ]
```