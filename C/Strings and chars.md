```c
#include "cs50.h"
#include <stdio.h>

int main(void) {
	char c1 = 'H';
	char c2 = 'I';
	char c3 = '!';

	string word = "HI!";

	printf("%c %c %c\n", c1, c2, c3); // H I !
	printf("%i %i %i\n", c1, c2, c3); // 72 73 33

	printf("%s\n", word); // HI!

	printf("%c %c %c\n", word[0], word[1], word[2]); // H I !
	printf("%i %i %i %i\n", word[0], word[1], word[2], word[3]); // 72 73 33 0
}
```

```c
#include "cs50.h"
#include <stdio.h>

int main(void) {
	string words[2];

	words[0] = "HI!";
	words[1] = "BYE!";

	printf("%c%c%c\n", words[0][0], words[0][1], words[0][2]); // indexing word from list at 0 idx and char of that word at idx's 0, 1, 2
	printf("%c%c%c%c\n", words[1][0], words[1][1], words[1][2], words[1][3]);
}
```