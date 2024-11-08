In C comparing strings directly won't work.
Using strings library will be needed for usage of a function for comparison.

```C
#include <stdio.h>
#include <strings.h>
#include "cs50.h"

int main(void) {
	string word = "pepe";
	string word2 = "pepe";

	# same strings will return 0
	if (chkstr(word, word2) == 0) {
		printf("Strings are the same %s %s \n", word, word2);
	}
}
```
[[Strings and chars]]