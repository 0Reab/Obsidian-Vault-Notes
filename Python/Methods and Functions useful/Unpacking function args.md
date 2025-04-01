If for example you'd like to pass in a list of something or tuple to let's say zip() function you will get `TypeError: unhashable type list`.
But the zip will work if  you simply pass in zip(x,y,z) but not in an list.

A workaround to this is to unpack these values using `zip(*array)`.
this will effectively do the same as zip(x,y,z) but you will have the benefit of dynamic python lists.