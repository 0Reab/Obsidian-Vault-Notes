```c
#include <stdio.h>
#include <string.h>
#include "cs50.h"

void draw(int n);

int main(void) {
 	int height = 4;
 	draw(height);
}


void draw(int n) {
 	if (n <= 0){
 		return;
 	}

 	draw(n - 1);

 	for (int i=0; i<n; i++) {
 		printf("#");
 	}
 	printf("\n");
}
```
```c
Output:
#
##
###
####
```
This is a recursive function.
You might intuitively expect the output to be basically reverse of what is presented.
After all n -1 is decreasing each time, meaning the for loop will print  "#" times n.

But until the code hits the return in the if statements (base case) nothing is returned or printed.
So what happens:
```c
// Until we hit the return function recursively calls itself
draw(n-1); // until n = 0
n-1 = 3
n-1 = 2
n-1 = 1
n-1 = 0 // time to return

// The function unwinds what happend in recursion to return all the "recursions"
// aka all the times n-1 happend
// and it happens in reverse aka unwinding

n = 1 // retrun for loops prinf with variable n set to 1
n = 2 // return ... with n as 2
n = 3 // as you can see we are reversing what was recursive
n = 4

// resulting in the pyramid
// #
// ##
// ###
// ####
```
