There is no dictionary data type in bash but associative array is basically an dictionary.

```bash
declare -A dict=(
	john=5
	alice=4
	bob=2
)

echo ${!dict[@]} 
```