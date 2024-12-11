![[Pasted image 20241107205220.png]]

Is similar to the Stack data struct, but the opposite.
You can imagine it as waiting in line at the grocery store.

It operates on the principle FIFO (first in first out).

We have front and rear of the queue.
And the operations we can use are enqueue which is arriving at the rear as last person in line.
Also dequeue as in, you just finished the checkout and you are leaving the store as a person in the front.

```python
from collections import deque  
from time import sleep  
import threading  
  
class Queue:  
    def __init__(self):  
        self.buffer = deque()  
  
    def enque(self, val):  
        self.buffer.appendleft(val)  
  
    def deque(self):  
        return self.buffer.pop()  
  
    def is_empty(self):  
        return len(self.buffer) == 0  
  
    def size(self):  
        return len(self.buffer)  
  
orders = ['pizza','samosa','pasta','biryani','burger']  
que = Queue()  
  
# place the order thread every 0.5 seconds  
# serve (print) order thread every 2 seconds  
  
def place_order(data_struct, ordered):  
    for i in ordered:  
        data_struct.enque(i)  
        print(f'enter q {i}')  
        sleep(0.5)  
  
def serve_order():  
    while not que.is_empty():  
        print('leave q', que.deque())  
        sleep(2)  
  
  
# define threads  
t1 = threading.Thread(target=place_order, args=(que, orders))  
t2 = threading.Thread(target=serve_order)  
  
t1.start()  
sleep(1)  
t2.start()  
  
# wait here until thread is done  
t1.join()  
t2.join()
```