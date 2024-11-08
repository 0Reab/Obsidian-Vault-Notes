```c
#include <stdio.h>
#include "cs50.h"

int main(void) {
	long x = 1;
	long y = 3;

	double z = (double) x / (double) y;

	prinf("%.20f\n", z);

	return 0;
}
```
In this example integers x and y are declared as long instead of int which is capable of 32 bits instead of 16 for int.

Type casting is performed on division as float is not precise enough compared to double.
float would return 0.3333236456 while double would start scrambling a bit further down the digits.

Lastly printing with %.20f prints first 20 digits of the floating point number.