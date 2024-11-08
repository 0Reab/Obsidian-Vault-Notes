```python
from os import mkdir, path
import os

structure = [ 'Lights', 'Flats', 'Processing']

dest = r"C:/Users/asd/Desktop/sublime_text_build_4169_x64/testera"

[ os.mkdir(os.path.join(dest,i)) for i in structure ]

```
How to make folders from structure list inside of dest path.
Os module takes care of forward slashes and a slash for joining two paths.
