```c
#include "cs50.h"
#include <stdio.h>

float calc(float bill, float tax, float tip);

int main(void) {
	float bill = 20;
	float tax = 25;
	float tip = 10;

	float my_half = calc(bill, tax, tip);

	printf("To split %f I need to give %f \n", bill, my_half);

	return 0;
}


float calc(float bill, float tax, float tip) {
	float taxed_bill = (tax / 100.0) * bill; 
	float taxed_tip = (tip / 100.0) * (taxed_bill + bill);

	float total = (taxed_bill + taxed_tip + bill) / 2;

	printf("Tax for bill %f\n Tip %f\n", taxed_bill, taxed_tip);
	printf("Total tax and bill %f\n", total);

	return total;
}
```