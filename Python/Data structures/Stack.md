![[Pasted image 20241107193813.png]]

Is a data structure that follows the principle of LIFO (last in first out).
Which is the opposite of que.

There are three main operations for the stack, **pop, peek, push**.

Pushing onto the stack, aka putting the item on top.
Popping out of the stack, aka removing the item on top.
Peeking on the stack, aka looking at the item on top.

```python
class Node:  
    def __init__(self, value):  
        self.value = value  
        self.next = None  
  
  
class Stack:  
  
    def __init__(self):  
        self.top = None  
        self.size = 0  
  
    def __len__(self):  
        return self.size  
  
    def __repr__(self):  
        items = []  
        curr_item = self.top  
  
        while curr_item is not None:  
            items.append(str(curr_item.value))  
            curr_item = curr_item.next  
  
        return ', '.join(items)  
  
  
    def err_empty(self):  
        raise ValueError("Stack is Empty")  
  
    def push(self, value):  
        new_node = Node(value)  
        new_node.next = self.top  
        self.top = new_node  
  
        self.size += 1  
  
    def pop(self):  
        if self.top is None:  
            self.err_empty()  
        else:  
            return_value = self.top.value  
            self.top = self.top.next  
            self.size -= 1  
            return return_value  
  
    def peek(self):  
        if self.top is None:  
            self.err_empty()  
        else:  
            return self.top.value  
  
    def is_empty(self):  
        return self.top is None  
  
if __name__ == "__main__":  
    stack = Stack()  
  
    stack.push(30)  
    stack.push(20)  
    stack.push(12)  
    stack.push(11)  
  
    print(stack)  
  
    print(stack.peek())  
  
    print(stack.pop())  
    print(stack)
```