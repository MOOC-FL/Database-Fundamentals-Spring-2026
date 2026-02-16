#### Example: Station name
- The following code asks the user for the station ID number and uses it to retrieve the station name from the database:
```R
library(RSQLite)

db <- dbConnect(SQLite(), "bikes_2024.db")

station_id <- readline(prompt = "Anna aseman id-numero: ")
result <- dbGetQuery(db,
                     "SELECT name FROM Stations WHERE id = ?",
                     params = list(station_id))

station_name <- result$name[1]
cat("Aseman nimi:", station_name, "\n")
```
-  The code execution might look like this:
```text
Anna aseman id-numero: 42
Aseman nimi: Haapaniemenkatu
```
- Here, the query contains a parameter `?` whose value is `station_id` the id number in the variable. The code shows how to `dbGetQuery` provide the parameter values ​​as a list when calling a function.
- Here, the syntax `result$name[1]` means that the column `name` value is retrieved in row 1 of the scoreboard, which in this case is the only row.
- If there is no station with the given id number in the database, the code returns the following result ( `NA` denotes a missing value):
```text
Anna aseman id-numero: 666
Aseman nimi: NA
```
- We can give a clearer message in this situation by, for example, checking the number of rows in the scoreboard:
```R
library(RSQLite)

db <- dbConnect(SQLite(), "bikes_2024.db")

station_id <- readline(prompt = "Anna aseman id-numero: ")

result <- dbGetQuery(db,
                     "SELECT name FROM Stations WHERE id = ?",
                     params = list(station_id))

if (nrow(result) == 1) {
  station_name <- result$name[1]
  cat("Aseman nimi:", station_name, "\n")
} else {
  cat("Asemaa ei löytynyt\n")
}
```
- Now the code gives a clear message that the drive was not found:
```text
Anna aseman id-numero: 666
Asemaa ei löytynyt
```
- Here is another implementation where the database search is performed `find_station_name` via a function. This function returns the name of the drive or the string --if the drive was not found.
```R
library(RSQLite)

db <- dbConnect(SQLite(), "bikes_2024.db")

find_station_name <- function(station_id) {
  result <- dbGetQuery(db,
                       "SELECT name FROM Stations WHERE id = ?",
                       params = list(station_id))
  if (nrow(result) == 1) result$name[1] else "--"
}

station_id <- readline(prompt = "Anna aseman id-numero: ")
station_name <- find_station_name(station_id)
cat("Aseman nimi:", station_name, "\n")
```
- Now the code works as follows:
```TEXT
Anna aseman id-numero: 42
Aseman nimi: Haapaniemenkatu
Anna aseman id-numero: 666
Aseman nimi: --
```




