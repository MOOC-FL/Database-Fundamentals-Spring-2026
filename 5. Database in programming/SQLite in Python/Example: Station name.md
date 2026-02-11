#### Example: Station name
- The following code asks the user for the station ID number and uses it to retrieve the station name from the database:
```py
import sqlite3

db = sqlite3.connect("bikes_2024.db")

station_id = input("Anna aseman id-numero: ")
result = db.execute("SELECT name FROM Stations WHERE id = ?", [station_id])
station_name = result.fetchone()[0]
print("Aseman nimi:", station_name)

```
The code execution might look like this:
```text
Anna aseman id-numero: 42
Aseman nimi: Haapaniemenkatu
```

