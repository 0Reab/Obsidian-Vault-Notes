```c
#include "cs50.h"
#include <stdio.h>

int main(void) {
	int n; // define integer n

	do {
		n = get_int("Enter a num: \n");
	}
	while (n < 1);
}
```

User input in bash [[User input]], [[Reading user input one liner]], [[Checking user input]].