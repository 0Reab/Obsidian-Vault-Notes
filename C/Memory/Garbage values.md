In this code snippet.
We allocate lots of memory for the array numbers.
But no integers were assigned.

So what happens if we loop over the array?
```c
int main(void) {
	int numbers[1024]; // init an array

	for (int i=0; i<1024; i++) {
		printf("%i\n", numbers[i]) // go thru arr indexes and print values
	}
}
```
What we get is *garbage values*.
These values are perhaps some values used by other parts of the program, some of the values are 0 because C initialized them to 0.
Similarly to when you initialize a variable with.
```c
int number;
```
C will probably assign the number 0 to the variable.
(I think better practice to do it yourself).
[[Memory]]