```c
#include <stdio.h>

int main(void) {
	int x = 4000000000; // 4B
	int y = 4000000000; // 4B

	printf("%i\n", x + y);
}

Output:
-589934592
```
How is this possible?

To represent an integer via 32-bits you encounter an integer overflow with this high numbers.
And what happens to get this result is say for example we have this integer in binary form:
(Imagine the first 0 is missing in each case.)
Using this power of two seq of numbers (8,4,2,1) we can count the bits by addition.
```
0000 = 0
0001 = 1
0010 = 2
0011 = 3
0100 = 4
0101 = 5
0110 = 6
0111 = 7
1000 = 8
```
Eventually getting to 4th bit to turn on 1000 = 8 we exceed 3 bits and what patter is left is just 000.
By this method the resulting integer appears to be completely wrong.

Using type long instead of int helps to get to big numbers but still possibly hit the limit of the limit of your memory.
