To make an array in bash:
```bash
delcare -a array=("apple" "banana" "orange")

echo ${array[@]}   # all array elements
echo ${#array[@]}  # number of array elements
echo ${array[0]}   # element at index 0
```