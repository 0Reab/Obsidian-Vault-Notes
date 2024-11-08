If you have a start and stop number and you would like to loop thru them, this is an easy solution.

```bash
start_num=5
stop_num=10

for i in $(seq $start_num $stop_num); do
	echo "$i"
done
```
