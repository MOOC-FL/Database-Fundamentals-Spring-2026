#### ORDER BY and expressions
- One could imagine that in the survey
```sql
SELECT * FROM Products ORDER BY 1;
```
- the rows are ordered `1` by the expression. Since the value of the expression is in every row `1`, **this would not produce any particular order**. However, this is not the case, but rather `1` orders the rows by the first column, `2` by the second column, etc. So this is an alternative way of expressing the column on which the order is based.
- However, if `ORDER BY` the expression in the `-part` is anything other than a single number (such as `RANDOM()`), the rows are arranged according to that expression.


