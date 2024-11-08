```python
class User:  
    def __init__(self, x, y):  
        self.name = x  
        self.user_id = y  
  
    def __repr__(self): # represent on object call  
        return f'{self.name}:{self.user_id}'  
  
asd = User(x='hami', y=12)  
  
print(asd)
```