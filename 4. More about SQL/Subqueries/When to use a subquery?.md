##### When to use a subquery?
- Quite often, a subquery is an alternative way to implement a query that could be done in some other way. For example, both of the following queries retrieve the names of the products in customer 1's shopping cart:
```sql
SELECT
  Products.name
FROM
  Products, Purchases
WHERE
  Products.id = Purchases.product_id AND Purchases.customer_id = 1;
```
```sql
SELECT
  name
FROM
  Products
WHERE
  id IN (SELECT product_id FROM Purchases WHERE customer_id = 1);

```
-  The first query is a typical two-table query, while the second query selects products using a subquery. **Which query is better?**
- The first query is better because this is the intended way to retrieve information from tables in SQL using references. The second query works as is, but it is different from what you are used to and your database system may not be able to execute it as efficiently.

- You should only use a subquery when there is a real reason to do so. If the query can be easily done with a multi-table query, this is usually a better solution.


