#### LIKE
- The expression `s LIKE p` is true if the string` s` matches the description `p`. Special characters can be used in the description: character` _` means any single character and character `%` means any number of any characters. For example, the query
```sql
SELECT * FROM Products WHERE name LIKE '%ri%';
```
- searches for products that contain the string “ri” as part of their name (such as turnip and celery).

