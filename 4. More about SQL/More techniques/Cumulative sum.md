#### Cumulative sum
- A useful skill in SQL is to be able to calculate a cumulative sum , i.e. the sum of the numbers in a column up to that row for each row. For example, consider the following table `Items`:
```text
id  value
--  -----
1   200  
2   100  
3   400  
4   100  
```
- We can calculate the cumulative sum using a query on two tables like this:
```sql
SELECT
  A.id, SUM(B.value)
FROM
  Items A, Items B
WHERE
  B.id <= A.id
GROUP BY
  A.id;

```
```text
id  SUM(B.value)
--  ------------
1   200         
2   300         
3   700         
4   800 
```
- The idea here is to calculate the sum Afor a row in the table and then Bsearch for all rows in the table that idhave a sum smaller than or equal to Athe row in the table. The desired sums can be calculated SUMusing the function after grouping.

- A similar technique can be used in other situations if we want to calculate a result that depends somehow on all the “smaller” rows in the table.
#### Nested aggregates
- Consider a situation where we want to find the largest number of movies that were released in the same year. For example, in the following table, Moviesthe desired result is 2, because two movies were released in 1940.
```text
id  name       release_year
--  ---------  ------------
1   Lumikki    1937        
2   Fantasia   1940        
3   Pinocchio  1940        
4   Dumbo      1941        
5   Bambi      1942 
```
- This seems a bit tricky, because we would have to nest queries `COUNT` that count the number of occurrences, and then queries `MAX` that find the largest value. However, SQL doesn't allow queries `SELECT MAX(COUNT(release_year))` like this.
- However, we can start with a query that groups movies by year and retrieves the number of movies in each group:
```sql
SELECT COUNT(*) FROM Movies GROUP BY release_year;
```
```text
COUNT(*)  
--------
1         
2         
1         
1       
```
- We still need to find the largest of these numbers, which can be done using a subquery. In this case, a convenient way is to use a subquery so that its result is `FROM` in the -part of the main query, in which case the subquery creates a table from which the main query retrieves information:
```sql
SELECT MAX(year_count) FROM (
  SELECT COUNT(*) year_count FROM Movies GROUP BY release_year
);
```
```text
MAX(year_count)
---------------
2       
```
- Could the problem be solved without a subquery? Yes, because we can sort the results from largest to smallest and select the first row of the scoreboard:
```sql
SELECT COUNT(*) AS year_count FROM Movies GROUP BY release_year
ORDER BY year_count DESC LIMIT 1;
```
```text
year_count
----------
2          
```

