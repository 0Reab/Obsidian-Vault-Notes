```c
#include <stdio.h>
#include "cs50.h"

int main(void) {
	int array[] = {3,4,1,2,5};
	int num = 5;

	for (int i=0; i<5; i++) {
		
		if (array[i] == num) {
			printf("Found %i\n", array[i]);
			return 0;
		}

	printf("%i\n", array[i]);
	}

	printf("%i Not found\n", num);
	return 1;
}
```
This algorithm has time complexity of O(n).
Where time is denoted by the big O and n as in number.
In this case n representing the number of elements of the array.
Which directly influences the time complexity of this algorithm aka how much time it takes.
[[Time complexity]]