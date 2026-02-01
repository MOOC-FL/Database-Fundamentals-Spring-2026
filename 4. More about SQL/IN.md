#### IN
- The expression `x IN (...)`is true if `x` any of the given values ​​is `true`. For example, the query
```sql
SELECT
  SUM(price)
FROM
  Products
WHERE
  name IN ('lanttu', 'nauris', 'selleri');
```
- searches for the combined price of turnips, turnips and celery.

