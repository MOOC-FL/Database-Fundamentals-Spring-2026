#### SQLite in Python
- The Python standard library includes a module sqlite3that allows you to access an SQLite database. The following code connects to the database and retrieves Stationsinformation from the table using SQL queries:
```python
import sqlite3

db = sqlite3.connect("bikes_2024.db")

result = db.execute("SELECT id, name FROM Stations WHERE id = 5")
print(result.fetchone())

result = db.execute("SELECT id, name FROM Stations ORDER BY id LIMIT 10")
print(result.fetchall())
```
- The code output is as follows:
```text
(5, 'Sepänkatu')
[(1, 'Kaivopuisto'), (2, 'Laivasillankatu'), (3, 'Kapteeninpuistikko'), (4, 'Viiskulma'), (5, 'Sepänkatu'), (6, 'Hietalahdentori'), (7, 'Designmuseo'), (8, 'Vanha kirkkopuisto'), (9, 'Erottajan aukio'), (10, 'Kasarmitori')]
```
- Here `db` is a database object through which SQL commands can be executed using the method `execute.` In this code, two `SELECT`commands are executed.
- The first `SELECT` command retrieves `Stations` the row with `id` number `5` from the table. Since the query returns one row, the method is used `fetchone`, which returns one row as a `tuple`.
- The second `SELECT`command retrieves `Stations` the first ten rows from the table. Now we use the method `fetchall`, which returns a `list` where each `tuple` corresponds to one row in the table.
> Database file location
- A typical problem with code that uses a database is that the database file is located in a different location on the computer than the code expects it to be. If you can't get the above code to work, this is probably the reason.
- If the database file referenced in the code does not exist, the code creates a new empty database file. Since there is no table in this database `Stations`, any attempt to retrieve information from the table will fail.










