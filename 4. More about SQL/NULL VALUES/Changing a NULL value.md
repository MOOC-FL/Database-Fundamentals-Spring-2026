#### Changing a NULL value
- The function `IFNULL(a, b)` returns the value `a` if `a` it does not exist `NULL`, and otherwise the value `b`:
```sql
sqlite> SELECT IFNULL(5, 0);
IFNULL(5, 0)
------------
5          
sqlite> SELECT IFNULL(NULL, 0);
IFNULL(NULL, 0)
---------------
0
```
- The above is a typical way to use the function: when the second parameter is 0, the function changes the possible `NULL` value to zero. This is useful, for example, `LEFT JOIN` in queries `SUM` with the function.
- The function `IFNULL` is not a `SQL` standard function and does not work in all databases. A more common standard function is `COALESCE(...)`, which is given a list of values. The function returns the first value in the list that is not NULL, or the value `NULL`if each value is NULL. If the function has two parameters, it works the same as `IFNULL`.
```sql
sqlite> SELECT COALESCE(1, 2, 3);
COALESCE(1, 2, 3)
-----------------
1              
sqlite> SELECT COALESCE(NULL, 2, 3);
COALESCE(NULL, 2, 3)
--------------------
2                 
sqlite> SELECT COALESCE(NULL, NULL, 3);
COALESCE(NULL, NULL, 3)
-----------------------
3                    
sqlite> SELECT COALESCE(NULL, NULL, NULL);
COALESCE(NULL, NULL, NULL)
--------------------------
NULL
```



