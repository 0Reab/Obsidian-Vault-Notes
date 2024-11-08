[[free]]
[[malloc]]
[[Pointers]]
[[Segmentation fault]]
Each memory spot of 1 byte (8bits).
Where four of them would equal to 32 bit.
Each of the memory spot is denoted with [[Hexadecimal]] value.

For the sake of disambiguation 0x is prefixed when naming memory addresses with hex.
For example 0x1, 0x2, 0x3... 0xF2 ,0xFF.

In C there are special operators.
& - for getting an memory address of a piece of data.

"star" - is the opposite it means go there.
Aka dereference operator, going to a specific memory address.

%p in printf allows for printing memory address and we call the address via &.
```c
#include <stdio.h>
#include "cs50.h"

int main(void) {
	int n = 50;
	printf("%p\n", &n);
}
```
Inversely we can dereference an memory address to access what is stored in that address via star.
```c
#include <stdio.h>

int main(void) {
    int n = 50;
    int *p = &n; // p = pointer of addr n

    printf("%i\n", *p); // notice *p is dereferencing what is on that mem addr
}
```
Example:
```c
#include <stdio.h>

int main(void) {
	char *s = "HI!";

	printf("%p\n", s); // print pointer of var s via %p    
}
```
Incrementing memory address via for loop.
```c
#include <stdio.h>

int main(void) {
	char *s = "HI!";

	for (int i=0; i<3; i++) {
		printf("%c\n", *(s+i)); // deref using * (s+i), which is mem addr + 1
	} // s is a mem addr, *s is a char in this case.
}
```

Additionally 
If you were to hardcode two strings as the same value.
```c
string i = "lol";
string j = "lol";
```
Comparing i == j will return true because string "lol" is stored in the same memory address.
To which i and j are pointing to.
On other hand input that comes from runtime like user input will be dynamically allocated.
And i will be pointing to one memory addr and j will be pointing to a different memory address.
Meaning if we do i == j comparison it will return false because memory addresses will be different.

What happens in hardcoded strings if they are the same the compiler will optimize the memory allocation and simply point both pointers to one memory address.