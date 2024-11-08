```c
#include "cs50.h"
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#include <stdbool.h>

// index = 0.0588 * letters - 0.296 * sentences - 15.8
// letters - average number of letters per 100 words
// sentences -average number of sentences per 100 words

int calculate(string text);


int main(void) {
	string text = "Average number of letters per 100 words. Is for variable letters.";
	int result = calculate(text);

	printf("%i\n", result);
}


int calculate(string text) {
	int words = 0;
	int letters = 0;
	int sentences = 0;

	bool in_word = false;

	for (int i=0, j=strlen(text); i<j; i++) {
		if (isalpha(text[i])) {
			in_word = true;
			letters++;
		}

		if (isspace(text[i])) {
			in_word = false;
			words++;
		}

		if (text[i] == '!' || text[i] == '?' || text[i] == '.') {
			sentences++;
		}
	}

	if (in_word) {
		words++;
	}

	float S = (float) sentences / words * 100;
	float L = (float) letters / words * 100;

	return (int) (0.0588 * L - 0.296 * S - 15.8);
}




```
