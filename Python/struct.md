```python
from struct import *  
  
# store as bytes  
  
packed_data = pack('iif', 6, 19, 4.64)  
print(packed_data)  
  
print(calcsize('i')) # byte size of type x  
print(calcsize('f'))  
print(calcsize('iif'))  
  
# Convert bytes back b'\x06\x00\x00\x00\x13\x00\x00\x00\xe1z\x94@'  
  
original_data = unpack('iif', packed_data)  
print(original_data)  
  
print(unpack('iif', b'\x06\x00\x00\x00\x13\x00\x00\x00\xe1z\x94@'))
```