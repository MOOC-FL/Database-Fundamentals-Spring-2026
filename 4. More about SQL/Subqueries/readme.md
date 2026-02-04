#### Subqueries
- A subquery is an expression within an SQL statement that returns a value based on a query. We can construct subqueries in the same way as regular queries, and use them to perform searches that would be difficult to perform otherwise.
> Example

- As an example, let's consider a situation where the database contains players' results in a table `Results`. We assume that the contents of the table are as follows:
```text
id  name      score
--  --------  -----
1   Uolevi    120  
2   Maija     80   
3   Liisa     120  
4   Aapeli    45   
5   Kaaleppi  115  
```
- We now want to find out the players who have achieved the highest score, so the query should return Uolevi and Liisa. We can achieve this with a subquery as follows:
```sql
SELECT
  name, score
FROM
  Results
WHERE
  score = (SELECT MAX(score) FROM Results);
```
- In this query, the subquery is `SELECT MAX(score) FROM Results`, which returns the largest result in the table, in this case the value 120. Note that the subquery should be written in parentheses so that it does not get mixed up with the main query.
#### Creating a subquery
- A subquery can appear almost anywhere in a query, and depending on the situation, it can return a single value, a list of values, or an entire table.
#### Subquery on a column
- The following query uses a subquery to create a third column that shows the difference between the player's score and the record score:
```sql
SELECT
  name, score, (SELECT MAX(score) FROM Results) - score
FROM
  Results;
```
```text
name      score  (SELECT MAX(score) FROM Results) - score
--------  -----  ----------------------------------------
Uolevi    120    0                                     
Maija     80     40                                    
Liisa     120    0                                     
Aapeli    45     75                                    
Kaaleppi  115    5  
```
- Because the expression that produces the scoreboard column is complex, the scoreboard can be made clearer by renaming the column:
```sql
SELECT
  name,
  score,
  (SELECT MAX(score) FROM Results) - score AS difference
FROM
  Results;
```
```text
name      score  difference
--------  -----  ----------
Uolevi    120    0         
Maija     80     40        
Liisa     120    0         
Aapeli    45     75        
Kaaleppi  115    5        
```
#### Subquery as a table
- In the following query, the subquery creates a table with the top three results. The sum of these results (120 + 120 + 115) is calculated in the main query.
```sql
SELECT
  SUM(score)
FROM
  (SELECT * FROM Results ORDER BY score DESC LIMIT 3);
```
```text
SUM(score)
----------
355
```
- Here, `LIMIT` limit the scoreboard so that it only contains the first three rows.
> Note that without the subquery we would get the wrong result:
```sql
SELECT SUM(score) FROM Results ORDER BY score DESC LIMIT 3;
```
```text
SUM(score)
----------
480     
```
- This scoreboard only has one row with the sum of all results (480). So what is at the end of the query `LIMIT 3` has no effect on the result.
#### Subquery as a list
- The following query retrieves the players whose score is in the top three. The subquery returns a list of scores INfor the -statement.
```sql
SELECT
  name
FROM
  Results
WHERE
  score IN (SELECT score FROM Results ORDER BY score DESC LIMIT 3);
```
```text
name
----------
Uolevi    
Liisa     
Kaaleppi  
```


