```python
class Stats:
    def __init__(self, health, attack_points, defense_points, name):
        self.health = health
        self.attack_points = attack_points
        self.defense_points = defense_points
        self.name = name

class Hero(Stats): # inherit from class Stats (init func)
    @property
    def damage(self): # decorator for property
        return self.attack_points // 2  # Calculate damage on-the-fly

    def showstats(self):
        print(f"Health: {self.health}, Name: {self.name}, Damage: {self.damage}, "
              f"Attack Points: {self.attack_points}, Defense Points: {self.defense_points}")

# Create a Hero instance and show stats
ironman = Hero(name="ironman", health=30, attack_points=50, defense_points=45)
ironman.showstats()

```
