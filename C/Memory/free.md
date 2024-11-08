[[Memory]]
[[Segmentation fault]]
Free is the opposite of [[malloc]].
It frees back the memory that you requested with malloc.
You need to specify which memory address you want to free.

By the order of programming gods whatever memory you request with malloc.
You need to give it back.

```c
free(t); // variable t
```

When the program terminates it will automagically free the memory.
But in case of a program that runs all the time you need to free it.