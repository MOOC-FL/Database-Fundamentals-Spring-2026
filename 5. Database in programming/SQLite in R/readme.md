#### SQLite in R
- In `R`, the SQLite database is typically accessed `RSQLite` using a library that can be installed with the command `install.packages("RSQLite")`. The following code connects to the database and retrieves `Stations`information from the table using SQL queries:
```R
library(RSQLite)

db <- dbConnect(SQLite(), "bikes_2024.db")

result <- dbGetQuery(db, "SELECT id, name FROM Stations WHERE id = 5")
print(result)

result <- dbGetQuery(db, "SELECT id, name FROM Stations ORDER BY id LIMIT 10")
print(result)
```
- The code output is as follows:
```TEXT
 id      name
1  5 Sepänkatu

   id               name
1   1        Kaivopuisto
2   2    Laivasillankatu
3   3 Kapteeninpuistikko
4   4          Viiskulma
5   5          Sepänkatu
6   6    Hietalahdentori
7   7        Designmuseo
8   8 Vanha kirkkopuisto
9   9    Erottajan aukio
10 10        Kasarmitori
```
- Here `db` is a database object through which `SQL` commands can be executed using the function `dbGetQuery`. In this code, two `SELECT`commands are executed.
- The first `SELECT`command retrieves Stationsthe row with id number 5 from the table. The second `SELECT`command retrieves `Stations` the first ten rows from the table.
> Database file location
- A typical problem with code that uses a database is that the database file is located in a different location on the computer than the code expects it to be. If you can't get the above code to work, this is probably the reason.
- If the database file referenced in the code does not exist, the code creates a new empty database file. Since there is no table in this database Stations, any attempt to retrieve information from the table will fail.
Here's the R equivalent of Python's `sqlite3` functionality, using the `RSQLite` package (part of the tidyverse's database interface):

####  **The Essential 5 Commands in R**

#### 1. **Connect to Database**
```r
library(RSQLite)

# Connect to a file (creates if doesn't exist)
conn <- dbConnect(SQLite(), "mydatabase.db")

# Or create in memory (temporary)
conn <- dbConnect(SQLite(), ":memory:")
```

#### 2. **No Explicit Cursor Needed**
```r
# In R, you don't need a cursor - you work directly with the connection
# The dbExecute() and dbGetQuery() functions handle this internally
```

#### 3. **Execute SQL Commands**
```r
# For single commands (INSERT, UPDATE, DELETE, CREATE)
dbExecute(conn, "SQL STATEMENT HERE")

# For queries that return data (SELECT)
dbGetQuery(conn, "SQL STATEMENT HERE")

# For multiple statements at once
dbExecute(conn, "
    CREATE TABLE table1 (...);
    CREATE TABLE table2 (...);
")

# For multiple rows of data
data <- data.frame(name = c("Alice", "Bob"), age = c(30, 25))
dbWriteTable(conn, "users", data, append = TRUE)
```

#### 4. **Save Changes (Commit)**
```r
# Most operations auto-commit in RSQLite
# But you can explicitly commit if using transactions
dbExecute(conn, "BEGIN TRANSACTION")
# ... do stuff ...
dbExecute(conn, "COMMIT")
```

#### 5. **Close Connection**
```r
dbDisconnect(conn)  # Always disconnect when done
```

#### 📋 **Complete Template for Any SQL Command**

```r
library(RSQLite)

# 1. CONNECT
conn <- dbConnect(SQLite(), "database.db")

# 2. EXECUTE YOUR SQL COMMAND
tryCatch({
    # For SELECT queries
    if (grepl("^SELECT", toupper(sql_command))) {
        result <- dbGetQuery(conn, sql_command)
        print(result)
    } 
    # For INSERT/UPDATE/DELETE/CREATE
    else {
        rows_affected <- dbExecute(conn, sql_command)
        print(paste("Rows affected:", rows_affected))
    }
    
}, error = function(e) {
    print(paste("Database error:", e$message))
    
}, finally = {
    # 3. ALWAYS DISCONNECT
    dbDisconnect(conn)
})
```

#### 🔄 **The 4 SQL Operations (CRUD) in R**

#### **CREATE (INSERT)**
```r
# Single row using parameterized query
dbExecute(conn, "INSERT INTO users (name, age) VALUES (?, ?)", 
          params = list('Alice', 30))

# Multiple rows using data frame
users_df <- data.frame(
  name = c("Bob", "Charlie"),
  age = c(25, 35)
)
dbWriteTable(conn, "users", users_df, append = TRUE)

# Create table from data frame (useful for quick imports)
dbWriteTable(conn, "new_table", mtcars)  # Creates table automatically
```

#### **READ (SELECT)**
```r
# Get all rows
result <- dbGetQuery(conn, "SELECT * FROM users")
print(result)  # Returns as data frame

# Get with parameters
result <- dbGetQuery(conn, 
                     "SELECT * FROM users WHERE age > ?", 
                     params = list(25))

# Get specific number of rows
result <- dbGetQuery(conn, "SELECT * FROM users LIMIT 3")

# Stream large results (doesn't load all at once)
res <- dbSendQuery(conn, "SELECT * FROM large_table")
while (!dbHasCompleted(res)) {
  chunk <- dbFetch(res, n = 1000)  # Fetch 1000 rows at a time
  process(ch chunk)
}
dbClearResult(res)
```

#### **UPDATE**
```r
rows_affected <- dbExecute(conn, 
                          "UPDATE users SET age = ? WHERE name = ?", 
                          params = list(31, 'Alice'))
print(paste("Updated", rows_affected, "rows"))
```

#### **DELETE**
```r
rows_affected <- dbExecute(conn, 
                          "DELETE FROM users WHERE age < ?", 
                          params = list(18))
print(paste("Deleted", rows_affected, "rows"))
```

#### 🎯 **Quick Reference: Most Used Patterns**

#### **Safe Parameter Passing (ALWAYS use this)**
```r
# GOOD - Prevents SQL injection
dbGetQuery(conn, 
          "SELECT * FROM users WHERE name = ? AND age = ?", 
          params = list('Alice', 30))

# BAD - Never do this!
dbGetQuery(conn, paste0("SELECT * FROM users WHERE name = '", name, "'"))  # DANGER!
```

#### **List All Tables**
```r
tables <- dbListTables(conn)
print(tables)
```

#### **Get Table Information**
```r
# Get column names and types
dbListFields(conn, "users")

# Get table structure/schema
dbGetQuery(conn, "PRAGMA table_info(users)")
```

#### **Check if Table Exists**
```r
table_exists <- "users" %in% dbListTables(conn)
```

#### **Get Last Inserted ID**
```r
dbExecute(conn, "INSERT INTO users (name) VALUES (?)", params = list('Dave'))
new_id <- dbGetQuery(conn, "SELECT last_insert_rowid()")[1,1]
print(paste("New user ID:", new_id))
```

#### **Count Rows**
```r
count <- dbGetQuery(conn, "SELECT COUNT(*) FROM users")[1,1]
```

#### 

```r
library(RSQLite)
library(DBI)  # For additional database functions

execute_sql <- function(sql, params = list(), fetch = FALSE) {
  """
  Safe SQL execution function
  Returns: data frame for SELECT, rows affected for others, or NULL on error
  """
  conn <- NULL
  tryCatch({
    conn <- dbConnect(SQLite(), "database.db")
    
    if (fetch || grepl("^SELECT", toupper(trimws(sql)))) {
      # For queries that return data
      result <- dbGetQuery(conn, sql, params = params)
      return(result)
    } else {
      # For INSERT/UPDATE/DELETE/CREATE
      rows_affected <- dbExecute(conn, sql, params = params)
      return(rows_affected)
    }
    
  }, error = function(e) {
    message(paste("Error:", e$message))
    return(NULL)
  }, finally = {
    if (!is.null(conn)) {
      dbDisconnect(conn)
    }
  })
}

# Usage examples:
# results <- execute_sql("SELECT * FROM users WHERE age > ?", list(25), fetch=TRUE)
# rows <- execute_sql("UPDATE users SET age = ? WHERE name = ?", list(30, 'Alice'))
```

#### 

```r
library(RSQLite)

# ❌ WRONG: Forgetting quotes in SQL
conn <- dbConnect(SQLite(), ":memory:")
dbExecute(conn, "CREATE TABLE users (name TEXT, age INTEGER)")
# Using string concatenation (BAD!)
name <- "Alice"
dbExecute(conn, paste0("INSERT INTO users VALUES ('", name, "', 30)"))  # SQL injection risk!

# ✅ RIGHT: Using parameterized queries
dbExecute(conn, "INSERT INTO users VALUES (?, ?)", params = list(name, 30))

# ❌ WRONG: Not disconnecting
conn <- dbConnect(SQLite(), "database.db")
# ... do stuff ...
# Connection stays open!

# ✅ RIGHT: Always disconnect
conn <- dbConnect(SQLite(), "database.db")
tryCatch({
  # ... do stuff ...
}, finally = {
  dbDisconnect(conn)  # This always runs
})

# ✅ OR use local() for automatic cleanup
with_connection <- function(code) {
  conn <- dbConnect(SQLite(), "database.db")
  on.exit(dbDisconnect(conn))
  force(code)
}

with_connection({
  dbExecute(conn, "INSERT INTO users VALUES ('Alice', 30)")
})

# ❌ WRONG: Loading entire large table into memory
all_data <- dbGetQuery(conn, "SELECT * FROM huge_table")  # May crash!

# ✅ RIGHT: Stream large results
res <- dbSendQuery(conn, "SELECT * FROM huge_table")
while (!dbHasCompleted(res)) {
  chunk <- dbFetch(res, n = 10000)  # Process 10k rows at a time
  # process chunk...
}
dbClearResult(res)
```

####  **R-Specific Features**

#### **Using with dplyr (tidyverse style)**
```r
library(dplyr)
library(DBI)

conn <- dbConnect(SQLite(), "database.db")

# Write a table
copy_to(conn, mtcars, "cars", temporary = FALSE)

# Work with database tables using dplyr verbs
cars_db <- tbl(conn, "cars")
cars_db %>%
  filter(mpg > 20) %>%
  select(mpg, cyl, hp) %>%
  arrange(desc(mpg)) %>%
  collect()  # Execute query and return results
```

#### **Transactions**
```r
# Start transaction
dbExecute(conn, "BEGIN TRANSACTION")

tryCatch({
  dbExecute(conn, "UPDATE accounts SET balance = balance - 100 WHERE id = 1")
  dbExecute(conn, "UPDATE accounts SET balance = balance + 100 WHERE id = 2")
  
  # If all successful, commit
  dbExecute(conn, "COMMIT")
}, error = function(e) {
  # If error, rollback
  dbExecute(conn, "ROLLBACK")
  print(paste("Transaction failed:", e$message))
})
```

#### This covers the main R patterns for working with SQLite! The key differences from Python are:
- No explicit cursor needed
- Functions return data frames directly
- `dbExecute()` for modifications, `dbGetQuery()` for SELECT
- Integration with dplyr for tidyverse-style database operations





