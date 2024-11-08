If a program uses more memory space than the stack size then stack overflow will occur and can result in a program crash.
There are two cases in which stack overflow can occur:

- 1. If we declare large number of local variables or declare an array or matrix or any higher dimensional array of large size can result in overflow of stack.

```c
// stack overflow by allocating large local memory
#include <stdio.h>
int main() {
	// create a matrix of 10^5 x 10^5
	// which may result in stack overflow
	int mat[100000][100000];
}
```

- 1. If function recursively call itself infinite times then the stack is unable to store large number of local variables used by every function call and will result in overflow of stack.
 
```c
// stack overflow via non terminating recursive function
#include <stdio.h

void fun(int x) {
	if (x == 1)
		return;
	x = 6;
	fun(x);
}

int main() {
	int x = 5;
	fun(x);
}
```
[[Memory]]
[[C/Memory/Stack]]
[[Pointers]]