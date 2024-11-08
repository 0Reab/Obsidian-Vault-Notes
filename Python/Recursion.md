Is when a function calls itself.
It can be useful to simplify code, although it might not have lower time complexity in most cases.
Here is some basic examples of recursion and explanations.
```python
# adding numbers from 1 to n
def adding(n):  
    if n == 0: # base case  
        return n  
    else:  
        return n + adding(n-1) # recursive call  
  
'''  
Explanation:  
On the first function call n=3, we hit the else block. - return n + adding(n-1)  
What happens here: 3 + 2.  
On this first recursive call n=2, we hit the else block. - return n + adding(n-1)  
What happens here: 2 + 1.  
On the second recursive call n=1, we hit the else block. return n + adding(n-1)  
What happens here: 1 + 0  
On the third recursive call n=0, we hit the if block now. - return n  
What happens here: it return 0 because n=0  
But  
The recursion starts to unwind the stack, like stacked pallets, all return statements/pallets will be retrieved:  
3rd call: returns 1 + 0 : adding(1)  
2nd call: returns 2 + 1 : adding(2)  
1st call: return 3 + 3 : adding(3) second number is 3 because what last return is which is 2+1 '''  

print(adding(3))
Output: 6
```

Example 2:
```python
def factorial(n):  
    if n == 1: # base case  
        return n  
    else:  
        return n * factorial(n-1)  
'''  
Similar to previous example  
We have a base case for final return and recursive else block.  
How it works:  
n = 5, we hit else block where function recurs factorial(4)  
n = 4, we hit else block where function recurs factorial(3)  
... until ...  
n = 1, we hit if block where we return n which is 1  
And the unwinding starts happening.  
1 * 2 = 2 in other words 1 from last return factorial(n-1) which will become the result of this expression on next occurance, and 2 from factorial(n-1)  
2 * 3 = 6  
6 * 4 = 24  
24 * 5 = 120  
And that is final return 120  
'''  
  
print(factorial(5)) # should be 120
```
Example 3:
```python

```