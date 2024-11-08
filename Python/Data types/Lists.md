Lists or arrays in python are a data type that contains elements.

```python
players = [ 11, 33, 44, 55]
print(players[0]) # 11

added_players = players + [66]
print(added_players) # 11, 33, 44, 55, 66

players[:2] = [0, 0]
print(players) # 0, 0, 44, 55
players[:2] = []
print(players) # 44, 55
```