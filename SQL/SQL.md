Stands for structured query language.

Meaning it is similar to a programming language in a sense.
Every sql database has DBSM - database management system.
And that can be Mysql, postgresql etc...

The database is basically an excel spreadsheet on steroids.

There are two types of databases, relational and non relational.
Relational databases, make relations to other databases to infer data and together they form a 
big database in some sense.
Other ones are i thing alone tables that do not have any relation ship to other tables.

So let's create a database using mariadb that comes with Kali.
```shell
sudo mysql
# if you run into some networking error such as can't connect
# try this to check and start the service
sudo systemctl status mysql
# if its not active
sudo systemctl start mysql
```

After this you should be in a new "shell" to interact with the dbsm.

For or first database let's name it something.
```shell
show databases # you will see some default databases
create database tutorial; # don't forget semicolons to finish your statements.
```

Now to enter into our DB and interact with it.
```shell
use tutorial; 
show tables; # display tables in the database
```

Let's create a table, we will need to specify some properties for the fields of the table.
```shell
create table first_table ( # press enter for new row
	id int, # def field named id that will contain an integer aka decimal number
	name varchar(255), # def field name that will contain string of max length 255
	region varchar(255),
	roast varchar(255) # press enter again
	); # close the first bracket we opened and ; to end statement
```

```shell
show tables; # display our tables in database called tutorial
describe first_table; # see how we setup the table and properties of each field.
```

Now let's populate our first table with some data.
```shell
insert into first_table values (1, "default route", "ethiopia", "light")
# following our tables properties we enter int for id field, and strings for our fields that expect varchar(255) as input.
```

Time for your first select statement.
```shell
select * form first_table; # this utilizes wildcard star symbol to encompass everything from the table.
select id from first_table; # display specifc column from the table
select name from first_table; # --||--
```

Let's say you have this table:
```shell
MariaDB [m_hacking]> select * from avengers;
+------+------------+-----------+----------+------+------------------+
| id   | first_name | last_name | origin   | age  | alias            |
+------+------------+-----------+----------+------+------------------+
|    1 | thor       | odinson   | asgard   | 1500 | strongestavenger |
|    2 | clint      | barton    | earth    |   35 | hawkeye          |
|    3 | tony       | stark     | earth    |   52 | iron man         |
|    4 | peter      | parker    | earth    |   17 | spiderman        |
|    5 | groot      | groot     | planet x |   18 | tree             |
+------+------------+-----------+----------+------+------------------+
```
And you wanted to see every avenger that is from earth.
```shell
select * from avengers where origin = "earth";
```
If you wanted to see every avenger from earth and asgard.
```shell
select * from avengers where origin = "erath" or origin = "asgard";
```

Some other example select statements.
```shell
select alias from avengers where ave < 30; # show alias if age < 30
select alias from avengers where not origin = "earth"; # alias if not from eart
```

Here is how to remove items from table.
```shell
delete from avengers where first_name = "jeff";
```

And updating.
```shell
update avengers set last_name = NULL where first_name = "groot";
```

Some cool ordering of data.
```shell
select * from avengers order by age desc; # or try asc for ascending/descending
```

What if we wanted to alter our table and add another column "beard" with true/false.
```shell
update avengers set beard = True where name = "thor"; 
```

Adding column to a table.
```shell
alter table test add another_column varchar(255)
```

removing table.
```shell
drop table test;
```

When creating a table you can force the table to always have values via not null,
make the id the primary key for uniq entries and make it automatically increment.
```shell
create table bands (
	id int not null auto_increment,
	name varchar(255) not null, # for example
	primary key (id)
);
```

```shell
MariaDB [record_company]> create table albums (
    -> id int not null auto_increment,
    -> name varchar(255) not null,
    -> release_year int,
    -> band_id int not null,
    -> primary key (id), 
    -> foreign key (band_id) references bands(id)
    -> );
```
To explain primary and foreign keys.
From the POV of table albums it's primary key is it's own ID of each albums unique identifier.
And each band in bands table has it's own uniq id.
So every albums foreign key will be band_id from bands table and primary key is id of an album form it's own albums table.

Using these keys we can make statements like in example, 

Example of multiple entries at once in table
```shell
insert into bands (name) values ("Deuce"), ("Avenged Sevenfold"), ("Ankor");
```

limit select statement
```shell
select * from bands limit 2;
```

Create alias names when using select, and for multiple columns.
```shell
MariaDB [record_company]> select id as 'ID', name as 'Band Name' from bands;
+----+-------------------+
| ID | Band Name         |
+----+-------------------+
|  1 | Iron Maiden       |
|  2 | Deuce             |
|  3 | Avenged Sevenfold |
|  4 | Ankor             |
+----+-------------------+
```

