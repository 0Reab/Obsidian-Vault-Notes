In case malloc tries to allocate memory that is used/is out of memory it will return NULL which we use to exit gracefully with code 1 instead of getting segmentation fault or whatever. 

To avoid memory leaks we free the memory we allocated with malloc. 

Heap and stack are two memory spaces, heap is for me the programmer to fiddle with using malloc and free and stack is for the os and compiler to fiddle with for function calls and what else. 

Assigning outside the allocated memory can also lead to segmentation fault or some other crazy stuff. 
Segmentation fault is an error when you try to access mem that you are not allowed to or in a inappropriate way. Please just confirm I at least grasped most of this stuff.

```c
#include <stdio.h>
#include <stdlib.h> // Don't forget this for malloc and free

int main(void) {
    int *x = malloc(3 * sizeof(int));
    // Allocates memory for an array of 3 integers (not bytes) on the heap.
    // `sizeof(int)` returns the number of bytes required for an integer on your platform,
    // typically 4 bytes on most systems. So, `malloc(3 * sizeof(int))` allocates 12 bytes.
    // The returned pointer `x` points to the first element of the array (starting address).
    // `x` itself is stored on the stack, while the allocated memory is on the heap.

    if (x == NULL) { // Checks if malloc failed to allocate memory.
        return 1;    // If malloc returns NULL, it indicates a memory allocation failure, 
                     // so we exit with code 1 to signal an error.
    }

    x[0] = 72; // Assigns 72 to the first element of the allocated memory block.
    x[1] = 73; // Assigns 73 to the second element of the allocated memory block.
    x[2] = 33; // Assigns 33 to the third element of the allocated memory block.

    // Attempting x[3] = 34; would result in undefined behavior because this tries
    // to access memory outside the allocated range, which can lead to a segmentation fault
    // or corrupt data if the memory happens to be accessible.

    free(x); // Frees the allocated memory, allowing the system to reclaim it.
             // After freeing, `x` still holds the address, but the memory is no longer yours
             // to use. It's good practice to set `x` to NULL after freeing to avoid accidental use.

    return 0; // Return 0 indicates successful program execution.
}

```