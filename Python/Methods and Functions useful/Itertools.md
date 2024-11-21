`Efficient looping module`

This is a list of explanations and most useful functions.

#### What is an iterator

Every function from itertools will return an iterator object, thus it's important to know what an iterator is.

So anything in python that is iterable, for example range().
We can call iter function on it. iter()
```python
r = range(1, 10)

print(r) # range(1, 10)
print(iter(r)) # returns object
```

```python
r = range(1, 10)
i = iter(r)

print(list(i)) # shows the range 1...9 non inclusive as usual
print(next(i)) # 1
print(next(i)) # 2
print(next(i)) # 3
```

#### Infinite iterators

```python
from itertools import count

def count_example(start, step):
	couter = count(start, step)

	for c in counter:
		print(c)

		if c == 100: break

count_example(10, 5)
```

```python
def repeat_example(element, max_repeats):
	repeater = repeat(element, max_repeats)

	for val in repeater:
		print(val)

repeat_example("hello", 10)
```

```python
def cycle_example(elements):
	i = 0
	cycler = cycle(elements)
	while i<100:
		print(next(cycler, end=' '))
		i += 1

cycle_example("ABCDEF")
```
#### Terminating iterators

```python
def accumulate_example(elements):
	running_sum = accumulate(elements)
	print(list(running_sum))

accumulate_example([1,2,3,4,5])
# returns [1,3,6,10,15]
```

```python
def chain_example(elements1, elements2):
		chained = chain(elements1, elements2)
		print(list(chained))

chain_example("ABC", "DEF")
# "ABCDEF"
```

```python
def chain_from_iterable_example(iterable):
	chained = chain.from_iterable(iterable)
	print(list(chained))

chain_from_iterable_examples([[1,2], [3,4], [5,6]])
# [1,2,3,4,5,6]
```

```python
def compress_example(data, selectors):
	compressed = compress(data, selectors)
	print(list(compressed))

compress_example([["a","b"], [1,2], [True,False]],
				[True, True, True]) # [["a","b"], [1,2], [True,False]]
				# [True, False, False]) [["a","b"]]
```

