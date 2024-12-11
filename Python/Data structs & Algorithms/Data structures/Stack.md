![[Pasted image 20241107193813.png]]

Is a data structure that follows the principle of LIFO (last in first out).
Which is the opposite of que.

There are three main operations for the stack, **pop, peek, push**.

Pushing onto the stack, aka putting the item on top.
Popping out of the stack, aka removing the item on top.
Peeking on the stack, aka looking at the item on top.

For python specific stack implementation.
Instead of using the basic list/array for the data, we should use `deque()` from collections.
The reason why is append and pop operations are O(n) while deque methods are O(1).
*deque - doubly ended queue*

```python
from collections import deque  
  
class Stack:  
    def __init__(self):  
        self.data = deque()  
  
    def peek(self):  
        return self.data[-1]  
  
    def pop(self):  
        return self.data.pop()  
  
    def push(self, item):  
        self.data.append(item)  
  
    def size(self):  
        return len(self.data)  
  
    def is_empty(self):  
        return len(self.data) == 0  
  
def reverse_string(s):  
    stack = Stack()  
    val = ''  
  
    for char in s:  
        stack.push(char)  
  
    while stack.size() != 0:  
        val += stack.pop()  
  
    return val
    ```