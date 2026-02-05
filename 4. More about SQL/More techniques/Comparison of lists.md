#### Comparison of lists
- Consider an array `Lists` that stores the contents of lists. For example, in the following array, list 1 contains the numbers [2, 4, 5], list 2 contains the numbers [3, 5], and list 3 contains the numbers [2, 4, 5]:
```text
id  list_id  value
--  -------  -----
1   1        2    
2   1        4    
3   1        5    
4   2        3    
5   2        5    
6   3        2    
7   3        4    
8   3        5    
```
 - The following query calculates for each pair of lists how many numbers they have in common :
```sql
SELECT
  A.list_id, B.list_id, COUNT(*)
FROM
  Lists A, Lists B
WHERE
  A.value = B.value
GROUP BY
  A.list_id, B.list_id;

```
```text
list_id  list_id  COUNT(*)
-------  -------  --------
1        1        3       
1        2        1       
1        3        3       
2        1        1       
2        2        2       
2        3        1       
3        1        3       
3        2        1       
3        3        3     
```
- This shows that, for example, lists 1 and 2 have one number in common (5) and lists 1 and 3 have three numbers in common (2, 4, 5). By extending such a query, we can compare whether two lists have exactly the same content. This is the case when the lists have the same number of numbers and the number of common numbers is equal to the number of numbers in a single list.

