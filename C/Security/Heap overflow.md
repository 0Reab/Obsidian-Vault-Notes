Happens when we continuously keep allocating memory and later on don't free that memory.
Resulting in memory leak - That memory is still being used but not freed for other processes.
```c
// heap overflow by continuously allocating mem 
#include <stdio.h>

int main(void) {
	for (int i=0; i<10000000; i++) {
		// allocate mem without freeing it
		int *ptr = (int *)malloc(sizeof(int));
	}
}
```
[[Memory]]
[[Heap]]
[[Pointers]]