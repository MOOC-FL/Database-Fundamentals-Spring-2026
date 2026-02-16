#### Test database
- The database contains the following two tables :
```sql
CREATE TABLE Stations (
    id INTEGER PRIMARY KEY,
    name TEXT
);

CREATE TABLE Trips (
    id INTEGER PRIMARY KEY,
    start_time TEXT,
    end_time TEXT,
    start_station_id INTEGER REFERENCES Stations,
    end_station_id INTEGER REFERENCES Stations,
    distance INTEGER,
    duration INTEGER
);
```
- The table `Stations` contains information about city bike stations. The table has two columns: `id`(id number) and `name`(station name).
- The table `Trips` contains information about trips. The table has the following columns:



