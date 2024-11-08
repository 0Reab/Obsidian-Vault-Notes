[[Memory]]
[[Segmentation fault]]
Is a function that allows you to request memory of certain size.
It will be granted to you by the OS.
The danger lies in the fact that there is no null termination so the responsibility is on you to manage it properly. [[Strings]]

```c
#include <stdio.h>
#include "cs50.h"
#include <ctype.h>
#include <stdlib.h>
#include <string.h>

int main(void) {

	char *s = get_string("Enter: ");   // get string
	char *t = malloc(strlen(s) + 1);   // allocate bytes to copy the first string + 1 for null character 

	for (int i=0; i<strlen(s) + 1; i++) {   // loop over string length + 1 for null character
		t[i] = s[i];                        // assign t at [i] (0,1...4) to s at [i]
	}										// the string s is copied to a different memory space called t
	// or just use strcpy(t, s); for short

	t[0] = toupper(t[0]); // capitalize first char

	printf("s: %s\n", s);
	printf("t: %s\n", t); // only t is capitalized beacuse two memory addresses instead of one to whom both pointers point to.

	printf("%p\n", s);
	printf("%p\n", t); // print pointers address
}
```
In case you ask for too much memory.
Using NULL which is a pointer not to be confused with nul from strings
which is forward slash zero. [[Strings]].

Here is how you can have some safety check in case you try to allocate more memory than is available to you.
```c
if (t == NULL) {
	return 1;
}
```