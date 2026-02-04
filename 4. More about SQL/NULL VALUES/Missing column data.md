#### Missing column data
- `NULL` One use of the value is to indicate that there is no data in a column. For example, in the following table, `Movies` the year of Dumbo is missing, so it is marked with `NULL`:
```text
id  name       release_year
--  ---------  ------------
1   Lumikki    1937        
2   Fantasia   1940        
3   Pinocchio  1940        
4   Dumbo      NULL        
5   Bambi      1942 
```
- When we first search for films from 1940 and then all films from other years, we get the following results:
```sql
SELECT * FROM Movies WHERE release_year = 1940;
```
```text
id  name       release_year
--  ---------  ------------
2   Fantasia   1940        
3   Pinocchio  1940  
```
```sql
SELECT * FROM Movies WHERE release_year <> 1940;
```
```text
id  name     release_year
--  -------  ------------
1   Lumikki  1937        
5   Bambi    1942    
```
- Since Dumbo doesn't have a year, we don't get it in either query, which is surprising. However, we can search for movies that don't have a year like this:
```sql
SELECT * FROM Movies WHERE release_year IS NULL;
```
```text
id  name   release_year
--  -----  ------------
4   Dumbo  NULL    
```















