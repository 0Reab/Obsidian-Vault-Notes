In bash indirect expansion can be used to indirectly get the value of some other variable.
Now what the hell does that mean.
Here is an example to help you understand.

```bash
name="john"
john="35"
```

In the example we have two variables, name and john.
notice how the name is assigned "john" and also john is another variable that equals to 35.

By using indirect expansion we can call variable $name and have it contain "35" instead of "john".

```bash
name="john"
john="35

#will output 35
echo ${!name}

#will output john
echo $name
```
