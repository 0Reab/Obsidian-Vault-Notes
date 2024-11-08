```python
from operator import attrgetter  
  
class User:  
    def __init__(self, x, y):  
        self.name = x  
        self.user_id = y  
  
    def __repr__(self): # represent on object call  
        return f'{self.name}:{self.user_id}'  
  
users = [  
    User('Mad', 34),  
    User('asdf', 54),  
    User('iaw', 53),  
]  
  
for user in users: print(user)  
  
print('----')  
  
for user in sorted(users, key=attrgetter('name')):  
    print(user)
```