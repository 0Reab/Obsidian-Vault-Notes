In C there are no strings as data types.
Strings are basically an array of chars like in [[Strings and chars]].

Strings are defined as.
```c
typedef char *string;
```
Which uses a star as a pointer to the first character of the soon to become string.
[[Pointers]]
The cs50 library that has string as a data type uses this type def to abstract away the crazy memory pointers to serve as training wheels.

Reason why the pointer only needs to point at the first char is the strings are \0 (null) terminated.
