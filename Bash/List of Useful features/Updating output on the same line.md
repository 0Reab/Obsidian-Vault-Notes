```bash
printf "\rAttempting: %s" "$password"
```
This example is from a password cracking bash script that loops over a wordlist.
Using this printf we can update the current password of the loop without cluttering our terminal with 15 million entries of rockyou.txt.

Keep in mind the quotes, there is two pairs.
One for the string and variable, %s indicates where the variable of the second string will be placed.