```c
#include <stdio.h>

int main(void) {
	for (int i=0; i<3; i++) {
		printf("Iteration %i\n", i);
	}
// alternatives
	int x = 0;
	while (x < 3) {
		x += 1; // or (x++) or (x = x + 1)	
		printf("Iteration %i\n", i);
	}

}
```