After creating our database we can add a table to it.

```sql
CREATE TABLE Person (name VARCHAR(50),
					age smallint UNSIGNED,
					personID int PRIMARY KEY AUTO_INCREMENT
					);--
```

So we created the table `Person`.

And for each person we will have columns that hold values.
- name - of length 50 variable characters.
- age - of small integer that is unsigned (will never exceed 100 and there's no need for negative numbers).
- personID - a unique identifier in case of duplicate names which is automatically incremented and is used as primary key.

Being specific with how you declare types of data can reduce it's size, especially when things start to scale.
The primary key will be used when we want to relate data from one table to another in later moment.

#### Display table and add data

Display the structure of the table Person.
```python
mycursor.execute('DESCRIBE Person')

for i in mycursor:
	print(i)
```

This will return tuples.

**Inserting**
```python
mycursor.execute("INSERT INTO Person (name, age) VALUES (%s,%s)", ('Tim', 19))
db.commit()
```
If you are familiar with C or string formatting, %s indicates a place where passed in tuple will be placing it's values in order.

Display the data from table.
```python
mycursor.execute('SELECT * FROM Person')

for i in mycursor:
	print(i)
	# ('Tim', 19, 1)
	# name - age - id
```