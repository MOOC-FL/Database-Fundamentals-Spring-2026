#### What to do anywhere?
- You can often do similar things on the database and code side. For example, here are two ways to find the longest path in a database:
```py
result = db.execute("SELECT MAX(distance) FROM Trips")
max_distance = result.fetchone()[0]
print(max_distance)
```
```py
result = db.execute("SELECT distance FROM Trips")
max_distance = max(result.fetchall())[0]
print(max_distance)
```
- The first method uses the SQL `MAX` function to find the longest distance on the database side. The second method uses the database to find the lengths of all distances in a list and then uses the Python `max` function to find the longest distance in the list on the code side.
- Of these two methods, the first method is clearly better: it is not good to retrieve unnecessary information in the code and perform processing that can also be easily done in the database.
- In particular, avoid executing multiple SQL commands unnecessarily, even if only one command would suffice. For example, the following is a bad way to retrieve the number of trips that started at each station from a database:
```python
result = db.execute("SELECT id, name FROM Stations")
stations = result.fetchall()

for station_id, station_name in stations:
    result = db.execute("""SELECT COUNT(*) FROM Trips
                           WHERE start_station_id = ?""",
                        [station_id])
    trip_count = result.fetchone()[0]
    print(station_name, trip_count)
```
- The code first retrieves the id number and name of each station from the list. After this, the code retrieves the number of trips that started at each station in a loop. The code works, but it does a lot of unnecessary work by retrieving the number of trips with a separate query. A better solution is to create a single query that retrieves everything needed directly:
```python
sql = """SELECT S.name, COUNT(*)
         FROM Stations AS S
           LEFT JOIN Trips AS T ON S.id = T.start_station_id
         GROUP BY S.id"""

data = db.execute(sql).fetchall()
for station_name, trip_count in data:
    print(station_name, trip_count)
```
- The resulting query is more complex, but it allows the database system to optimize the overall retrieval of the desired information and deliver the information to the code as efficiently as possible.


