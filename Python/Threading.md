```python
import threading

class Messages(Threading.Thread): # inherit from threading
	def run(self): # threading run() function
		for _ in range(10):
			print(threading.currentThread().getName())

x = Messages(name='Send out message')
y = Messages(name='Recieve messages')

x.start()
y.start()

```