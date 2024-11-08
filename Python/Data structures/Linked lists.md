![[Pasted image 20241107171559.png]]
Linked lists are a data structure that have nodes (10, 20, 30) and null pointers at the end.
Time complexity of this structure is O(n) which is linear, so in worst case you will go thru the whole data.

Linked lists hold data in the nodes, these objects - **nodes hold the data and the reference to the next node** in the list (hence the name linked lists).
This data struct is used often because of it's efficient insertion and deletion.

For double linked lists those references to next nodes are both ways (like in the diagram, white arrows).

```python
  
class Node: # define object node with: val and next  
  
    def __init__(self, value):  
        self.value = value  
        self.next = None  
  
  
class LinkedList:  
  
    def __init__(self):  
        self.head = None  
  
    def __repr__(self):  
        if self.head is None:  
            return "[]"  
        else:  
            last = self.head  
  
            return_string = f"[{last.value}"  
  
            while last.next:  
                last = last.next  
                return_string += f", {last.value}"  
            return_string += "]"  
  
            return return_string  
  
    def __contains__(self, value):  
        last = self.head  
        while last is not None:  
            if last.value == value:  
                return True  
            last = last.next  
        return False  
  
    def __len__(self):  
        last = self.head  
        counter = 0  
        while last is not None:  
            counter += 1  
            last = last.next  
        return counter  
  
    def idx_err(self):  
        raise ValueError("Idx out of bounds")  
  
    def append(self, value):  
        if self.head is None:  
            # if there is no head, define head as node  
            self.head = Node(value)  
        else:  
            # if there is head, traverse list until None  
            # and define last            last = self.head  
            while last.next:  
                last = last.next  
            last.next = Node(value)  
  
    def prepend(self, value):  
        first_node = Node(value)  
        first_node.next = self.head  
        self.head = first_node  
  
    def insert(self, value, index):  
        if index == 0:  
            self.prepend(value)  
        else:  
            if self.head is None:  
                self.idx_err()  
            else:  
                last = self.head  
  
                for i in range(index-1):  
                    if last.next is None:  
                        self.idx_err()  
                    last = last.next  
  
                new_node = Node(value)  
                new_node.next = last.next  
                last.next = new_node  
  
    def delete(self, value):  
        last = self.head  
  
        if last is not None:  
            if last.value == value:  
                self.head = last.next  
            else:  
                while last.next:  
                    if last.next.value == value:  
                        last.next = last.next.next  
                        break  
                    last = last.next  
    def pop(self, index):  
        if self.head is None:  
            self.idx_err()  
        else:  
            last = self.head  
  
            for i in range(index-1):  
                if last.next is None:  
                    self.idx_err()  
                last = last.next  
  
            if last.next is None:  
                self.idx_err()  
            else:  
                last.next = last.next.next  
  
    def get(self, index):  
        if self.head is None:  
            self.idx_err()  
        else:  
            last = self.head  
  
            for i in range(index):  
                if last.next is None:  
                    self.idx_err()  
                last = last.next  
            return last.value  
  
if __name__ == "__main__":  
    ll = LinkedList()  
  
    ll.append(2)  
    ll.append(12)  
    ll.append(13)  
    ll.append(30)  
  
    ll.prepend(1000)  
    ll.insert(200, 1)  
    #ll.delete(13)  
    ll.pop(1)  
    print(ll.get(1))  
    print(ll)  
    print(13 in ll)
```