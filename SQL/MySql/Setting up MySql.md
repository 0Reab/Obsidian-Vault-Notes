1. Download executable installer from mysql website.
2. Select full install (it's for development purposes).
3. Create password for admin user.

All the rest of configuration from installer can remain default.

### Testing with python

1. Open your terminal (cmd/powershell) and run `python` interpreter.
2. Enter `import mysql` and press enter.

If no errors occur you are set and ready.
If you get errors, most likely the connector for SQL and python is not installed.

`pip install mysql-connector-python`

Then try step 1 and 2 again.
Maybe you will need to do pip install with just `mysql`.

Now in your actual project that you plan to use sql.
```python
import mysql.connector

db = mysql.connector.connect()
```
If this runs with error no attribute connect, go to mysql website and install connector.
This can happen if pip installer didn't workout.

Boilerplate code to test connecting to DB.
```python
import mysql.connector

db = mysql.connector.connect(
	host="localhost",
	user="root", # security best practises 101
	passwd="root"
	)
```

Now to create a database add these two lines.
```python
mycursor = db.cursor()
nycursor.execute("CREATE DATABASE testdatabase")
```

Once you run the code with this query to create a database you can specify DB name in the mysql.connector.connect() parameters (databse="testdatabase").

