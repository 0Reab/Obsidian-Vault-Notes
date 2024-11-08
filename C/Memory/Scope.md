[[Memory]]
[[Garbage values]]
[[Pointers]]
```c
void swap(int a, int b);

int main(void) {
	int x = 3;
	int y = 2;

	printf("x = %i, y = %i\n", x, y);
	swap(x, y);
	printf("x = %i, y = %i\n", x, y);
}	

void swap(int a, int b) {
	int tmp = a;
	a = b;
	b = tmp;
}
Output:
x = 3, y = 2
x = 3, y = 2
```
This code snippet at first glance looks like it will work properly.
Swapping the value of x and y.
The reason the values don't get swapped is because the scope of the function swap.

Upon the call of function swap, values from x and y get copied into variables a and b to a function scope.
Which are effectively two different memory addresses.
And when the swap function is called it assigns a and b variables properly but x and y remain unchanged, and whatever memory the function occupied is now freed up and becomes [[Garbage values]].

Here is the correct code.
In this example we use pointers to memory addresses of original x and y variables.
Instead of making a copy of x and y values into a new variables on a new addresses.
```c
void swap(int *a, int *b);

int main(void) {
	int x = 3;
	int y = 2;

	printf("x = %i, y = %i\n", x, y);
	swap(&x, &y);
	printf("x = %i, y = %i\n", x, y);
}	

void swap(int *a, int *b) {
	int tmp = *a;
	*a = *b;
	*b = tmp;
}
Output:
x = 3, y = 2
x = 2, y = 3
```