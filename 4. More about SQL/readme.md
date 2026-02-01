#### 4. More about SQL
##### Types and expressions
- SQL uses types and expressions in the same way as programming. We've already seen many examples of SQL commands, but now is a good time to take a deeper look at the structure of the language.

- Each database system implements types and expressions in its own way, and there are many subtle differences in how databases work. So it's a good idea to check the documentation for the database you're using for details. 
##### Types
- In the table definition, each column is given a type:
```sql
CREATE TABLE Movies (
  id INTEGER PRIMARY KEY,
  name TEXT,
  release_year INTEGER
);
```
- Here, the column `name` type is `TEXT`(string) and the column `release_year` type is `INTEGER`(integer). These are the most common types available with these names in many databases. Examples of other common types include `TIMESTAMP`(timestamp), `REAL`(floating point), and `BLOB`(raw data).
> ##### TEXT vs. VARCHAR
> The traditional way to store a string in SQL is to use the type `VARCHAR`, which specifies the maximum length of the string in parentheses. For example, type `VARCHAR(10)`means that the string can have a maximum of 10 characters.
> 
> This is a throwback to old programming, where a string could be represented as a fixed-length array of characters. TEXTHowever, a type is more convenient because you don't have to figure out a maximum length.

#### Types of SQLite
- A special feature of SQLite is that the type in the table definition is just a guideline for what type the column should be. However, we can ignore the guideline and, for example, store a string instead of an integer:
```sql
INSERT INTO Movies (name, release_year) VALUES ('Lumikki', 'abc');
```
- Additionally, the type name can be any string, even if SQLite does not have such a type. This allows us to, for example, define a column in which to store a time:
```sql
CREATE TABLE Bookings (
  id INTEGER PRIMARY KEY,
  start_time TIMESTAMP,
  end_time TIMESTAMP,
  description TEXT
);
```
- SQLite does not have a type `TIMESTAMP`, instead times are treated as strings, but here the type of the column indicates what is intended to be stored in it.




