```c
#include <stdio.h>
#include <string.h>
#include "cs50.h"

int main(void) {
	string names[] = {"peter", "mark"};
	string nums[] = {"123", "456"};

	string name = "peter";

	for (int i=0; i<2; i++) {
		printf("%i\n", i);
		if (strcmp(name, names[i]) == 0) {
			printf("Found %s\n", nums[i]);
			return 0;
		}
	}
	printf("Not found\n");
	return 1;
}
```
In this example it would be beneficial to construct a data type to represent names and numbers.

```c
#include <stdio.h>
#include <string.h>
#include "cs50.h"

typedef struct {
	string name;
	string number;
}
person;

int main(void) {
	person people[2];

	people[0].name = "peter";
	people[0].number = "123";

	people[1].name = "mark";
	people[1].number = "456";

	string name = "peter";

	for (int i=0; i<2; i++) {
		printf("%i\n", i);
		if (strcmp(name, people[i].name) == 0) {
			printf("Found %s\n", people[i].number);
			return 0;
		}
	}
	printf("Not found\n");
	return 1;
}
```