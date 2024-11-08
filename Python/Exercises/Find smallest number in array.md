You are given an _odd-length_ array of integers, in which all of them are the same, except for one single number.

Complete the method which accepts such an array, and returns that single different number.

**The input array will always be valid!** (odd-length >= 3)

## Examples

```
[1, 1, 2] ==> 2
[17, 17, 3, 17, 17, 17, 17] ==> 3
```


Scroll down for solution...
/
/
/
/
/
/
/
/
/
/
/
/
/
/
/
/
/
/

```python
def smol(arr):

    count_dict = {}

    for i in arr:
        if i in count_dict:
            count_dict[i] += 1
        else:
            count_dict[i] = 1

    for k, v in count_dict.items():
        if v == 1:
            return k

print(smol([17, 17, 3, 17, 17, 17, 17]))
```
```python
def smol(arr):
	return [ i for i in arr if arr.count(i) == 1 ][0]

```

One liner solution 0 index at the end returns just the first element or resulting list.