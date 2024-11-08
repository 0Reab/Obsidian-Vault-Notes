```c
// Make this mario pyramid
   #  #
  ##  ##
 ###  ###
####  ####
```
```c
#include <stdio.h>
#include "cs50.h"

int main(void) {
	int height = 0;

	do {
		height = get_int("Enter a num (1-8): \n");
	} while (height > 8 || height < 1);

	for (int i = 1; i <= height; i++) {

		for (int g = height; g > i; g--) { // leftmost spaces
			printf(" ");
		}

		for (int z = 1; z <= i; z++) { // first pyramid
			printf("#");
		}

		printf("  "); // gap between pyramids

		for (int j = 1; j <= i; j++) { // right pyramid
			printf("#");
		}

		printf("\n");
	}

	return 0;
}
```