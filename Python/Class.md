```python
class Person:
    def __init__(self, name):
        self.name = name

    def talk(self):
        print(f"Hi, I am {self.name}")


mike = Person("Mike Tyson")
mike.talk()

bob = Person("Bob the builder")
bob.talk()
```

```python
class Mammal:
    def walk(self):
        print("walk")


class Dog(Mammal):
    def bark(self):
        print("bark")


class Cat(Mammal):
    def meow(self):
        print("meow")


dog1 = Dog()
dog1.walk()

cat1 = Cat()
cat1.walk()

dog1.bark()
cat1.meow()
```