```c
#include <stdio.h>
#include <stdbool.h>
#include <ctype.h>
#include "cs50.h"
#include <string.h>

// char check(char letter, char special);

int main(void) {
	char alphabet[] = {'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z'};
	int idx;

	int num = 2;

	string words = "";
	string plain_text = "abcd1234"; 
	string error_msg = "%s must be a postive integer\n";

	if (isalpha(num)) {
		printf("%s %s", error_msg, num);
		return 1;
	}

	else if (num < 0){
		printf("%s %s", error_msg, num);
		return 1;
	}

	if (num > 26) {
		num = num % 26;
	}

	for (int i=0, j=strlen(plain_text); i<j; i++) {
		printf("i = %i\n", i);

		if (isalpha(plain_text[i])) {
			idx = plain_text[i] - 'a';
			printf("idx calc = %i\n", idx);
			idx = idx + num;
			words += alphabet[idx];
			printf("curr words status = %s\n", words);
		}
		else {
			words += plain_text[i];
		}
	}	

	printf("%s\n", words);

	return 0;
}

```