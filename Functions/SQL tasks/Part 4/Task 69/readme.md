#### Task 69
- Calculate how many ways you can choose two products so that the total price is exactly 10.

> Note! There may be two of the same products in a combination and you want to calculate different combinations. In the example, the combinations are celery+celery and lenttu+carrot (i.e. carrot+lenttu is not included).
#### Example
```text
Input
Products
id	name	price
1	celery	5
2	swede	8
3	nauru	6
4	carrot	2
```
```text
Output
2
```
#### Solutions

**Restating the problem**  
We have a table of products with their IDs, names, and prices.  
We need to count **unique combinations** of **two products** (can be the same product) such that the **total price** of the two is exactly 10.  

"Combinations" here means: order doesn’t matter — `(celery, carrot)` is the same as `(carrot, celery)`.  

We need to return just the integer count of such pairs.  



#### Example from the problem

Input table:
```
id	name	price
1	celery	5
2	swede	8
3	nauru	6
4	carrot	2
```

#### Step 1: List all possible price pairs (allow same product)

Possible combinations of two products from the 4 available:  

The allowed products are (5, 8, 6, 2).

**Case 1 — same product**:  
- 5 + 5 = 10 ✅  
- 8 + 8 = 16 ❌  
- 6 + 6 = 12 ❌  
- 2 + 2 = 4 ❌  
→ (celery, celery) is valid.  

**Case 2 — different products**:  

Sorted prices: 2, 5, 6, 8  

Possible pairs with price sum = 10:  

- 2 + 8 = 10 ✅ → (carrot, swede)  
- 5 + 6 = 11 ❌  
- 2 + 6 = 8 ❌  
- 5 + 8 = 13 ❌  
- 2 + 5 = 7 ❌  
- 6 + 8 = 14 ❌  

Actually I need to be systematic with price pair (a, b) such that a ≤ b:  

From the set {2, 5, 6, 8}:  

(2, 2) → 4 ❌  
(2, 5) → 7 ❌  
(2, 6) → 8 ❌  
(2, 8) → 10 ✅  
(5, 5) → 10 ✅  
(5, 6) → 11 ❌  
(5, 8) → 13 ❌  
(6, 6) → 12 ❌  
(6, 8) → 14 ❌  
(8, 8) → 16 ❌  

So valid: (2, 8) and (5, 5).  


#### Step 2: Convert to product names

(2, 8) → carrot, swede  
(5, 5) → celery, celery  

That's 2 combinations.



#### Step 3: Verify uniqueness

No other combination sums to 10.

So the answer is **2**.



