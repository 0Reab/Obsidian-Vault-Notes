Just like in [[min() & max()]] we use the same method but with additional key such as 'price' or whatever to sort by that parameter in dictionary.

for example:
```python
inventory = {  
    'apple': {  
        'amount':5,  
        'price': 0.50  
    },  
    'orange': {  
        'amount':6,  
        'price': 0.55  
    }  
}
```

If this is our inventory and the task is to sort by price.
```python
def sort_price():
	return sorted(inventory.items(), key=lambda item: item[1]['price'])
```

Here is how it works... We are sorting a dictionary by looping thru each item in inventory.
The each item such as apple comes as tuple with it's parameters like price because we used inventory.items().
The key of sorting function is another lambda function which takes current item (the tuple of item name and it's parameters), so we take item at index 1 which is the parameters of each item.
In our case 'price' to get it's value and use that to sort for.

For visualization the tuple item looks like this ('apple', {'amount':5, 'price':0.50})
So item index 1 is 'apple' and index 2 is {'amount':5, 'price':0.50}
