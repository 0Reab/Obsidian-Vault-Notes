```python
import json

# func to save inventory to a file  
def save_inventory():  
    with open('inventory.json', 'w') as file:  
        json.dump(inventory, file)  
  
# func to load inventory from a file  
def load_inventory():  
    global inventory  
    with open('inventory.json', 'r') as file:  
        inventory = json.load(file)
```