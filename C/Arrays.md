```c
#include <stdio.h>
#include "cs50.h"

const int N = 3;

float average(int array[], float n);

int main(void) {

	int scores[N];

	for (int i = 0; i < 3; i++) {
		scores[i] = get_int("Scores: ");
	}

	printf("Avrg: %f\n", average(scores, N));
}


float average(int array[], float n) {
	int sum = 0;

	for (int i=0; i < n; i++) {
		sum += array[i];
	}
	return sum / n;
}
```