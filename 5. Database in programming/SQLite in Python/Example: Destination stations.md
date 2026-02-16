#### Example: Destination stations
-  The following code asks the user for the starting station and date, and then finds all the destination stations where a journey that started from the starting station ended on the given date.
```python
import sqlite3

db = sqlite3.connect("bikes_2024.db")

def find_destinations(station_name, date):
    sql = """SELECT
                 DISTINCT B.name
             FROM
                 Stations AS A, Stations AS B, Trips AS T
             WHERE
                 T.start_station_id = A.id AND
                 T.end_station_id = B.id AND
                 A.name = ? AND
                 T.start_time LIKE ?
             ORDER BY
                 B.name"""
    result = db.execute(sql, [station_name, date + "%"])
    return [row[0] for row in result.fetchall()]

station_name = input("Anna aseman nimi: ")
date = input("Anna päivämäärä: ")

destinations = find_destinations(station_name, date)
print("Kohteiden määrä:", len(destinations))
for destination in destinations:
    print(destination)
```
- Here is an example of how the code works:
```text
Anna aseman nimi: Syystie
Anna päivämäärä: 2024-05-16
Kohteiden määrä: 5
A.I. Virtasen aukio
Ala-Malmin tori
Huhtakuja
Pukinmäen asema
Vanha Tapanilantie
```
- The SQL command required here is complex, which is why it is split across multiple lines in the code. Here is a handy Python `"""`syntax that allows you to define a multi-line string.
- The command is given two parameters, which are placed `?` at the characters in the order they are given in the list. The first element of the list goes ?at the first character and the second element goes `?` at the second character. Since the parameters are strings, they are placed `'` inside the characters in SQL.
- Trips starting on a specific day can be found `LIKE`using the syntax by restricting the search so that the column `start_time` value must begin with the given date. The character `%`indicates that any time can follow the date.


