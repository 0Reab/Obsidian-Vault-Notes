![[Pasted image 20241107205220.png]]

Is similar to the Stack data struct, but the opposite.
You can imagine it as waiting in line at the grocery store.

It operates on the principle FIFO (first in first out).

We have front and rear of the queue.
And the operations we can use are enqueue which is arriving at the rear as last person in line.
Also dequeue as in, you just finished the checkout and you are leaving the store as a person in the front.

```python
class Node:  
    def __init__(self, value):  
        self.value = value  
        self.next = None  
  
  
class Queue:  
    def __init__(self):  
        self.front = None  
        self.rear =  None  
        self.size = 0  
  
    def __len__(self):  
        return self.size  
  
    def __repr__(self):  
        items = []  
  
        current = self.front  
  
        while current is not None:  
            items.append(str(current.value))  
            current = current.next  
      
        return ', '.join(items)  
  
    def err_empty(self):  
        raise IndexError('Queue is empty')  
  
    def enqueue(self, value):  
        new_node = Node(value)  
  
        if self.rear is None:  
            self.front = self.rear = new_node  
        else:  
            self.rear.next = new_node  
            self.rear = new_node  
  
        self.size += 1  
  
    def dequeue(self):  
        if self.front is None:  
            self.err_empty()  
  
        dq_val = self.front.value  
  
        self.front = dq_val.next  
  
        if self.front is None:  
            self.rear = None  
  
        self.size -= 1  
  
  
    def peek(self):  
        if self.front is None:  
            self.err_empty()  
  
        return self.front.value  
  
    def is_empty(self):  
        return self.front is None
```