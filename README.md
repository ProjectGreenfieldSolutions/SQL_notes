
### List of relevant SQL topics (PostgreSQL focused)

---

**`EXPLAIN`**  
Shows the query plan to help understand how SQL will execute a query.  
**Example:**  
```sql
EXPLAIN SELECT * FROM orders WHERE customer_id = 5;
```

---

**Sequence Scans**  
A full table scan; reads all rows when no index is used.  
**Example:** Happens when filtering on a non-indexed column.

---

**`CREATE INDEX`**  
Improves query speed by indexing specific columns.  
**Example:**  
```sql
CREATE INDEX idx_customer_name ON customers(name);
```

---

**`CREATE INDEX USING HASH`**  
Creates a hash-based index, best for equality comparisons.  
**Example:**  
```sql
CREATE INDEX idx_email_hash ON users USING HASH(email);
```

---

**`CREATE INDEX USING BTREE`**  
Creates a B-tree index, ideal for range and equality lookups.  
**Example:**  
```sql
CREATE INDEX idx_order_date ON orders USING BTREE(order_date);
```

---

**`CREATE TRIGGER`**  
Defines an automatic action in response to a DB event.  
**Example:**  
```sql
CREATE TRIGGER log_order
AFTER INSERT ON orders
FOR EACH ROW
INSERT INTO audit_log (order_id, action)
VALUES (NEW.id, 'INSERT');
```

---

**`CREATE FUNCTION`**  
Defines a reusable SQL or procedural code block.  
**Example:**  
```sql
CREATE FUNCTION get_discount(price DECIMAL)
RETURNS DECIMAL AS $$
BEGIN
  RETURN price * 0.9;
END;
$$ LANGUAGE plpgsql;
```

---

**`LEFT JOIN`**  
Returns all records from the left table and matched records from the right.  
**Example:**  
```sql
SELECT * FROM customers
LEFT JOIN orders ON customers.id = orders.customer_id;
```

---

**`RIGHT JOIN`**  
Returns all records from the right table and matched from the left.  
**Example:**  
```sql
SELECT * FROM orders
RIGHT JOIN customers ON customers.id = orders.customer_id;
```

---

**`INNER JOIN`**  
Returns records with matches in both tables.  
**Example:**  
```sql
SELECT * FROM orders
INNER JOIN customers ON orders.customer_id = customers.id;
```

---

**`FULL JOIN`**  
Combines LEFT and RIGHT JOIN, returns all with matches or NULLs.  
**Example:**  
```sql
SELECT * FROM customers
FULL JOIN orders ON customers.id = orders.customer_id;
```

---

**`WITH` (Common Table Expressions)**  
Defines temporary result sets to simplify complex queries.  
**Example:**  
```sql
WITH recent_orders AS (
  SELECT * FROM orders
  WHERE order_date > CURRENT_DATE - INTERVAL '7 days'
)
SELECT * FROM recent_orders
WHERE total > 100;
```
---

**`CASE WHEN`**  
A conditional expression to return values based on logic, like an inline `IF/ELSE`.  
**Why it's handy:** Great for categorizing or transforming data based on conditions.  
**Example:**  
```sql
SELECT 
  name,
  price,
  CASE 
    WHEN price > 100 THEN 'Expensive'
    WHEN price BETWEEN 50 AND 100 THEN 'Moderate'
    ELSE 'Cheap'
  END AS price_category
FROM products;
```
---

**`COALESCE`**  
Returns the first non-null value from a list of expressions.  
**Why it's handy:** Useful for handling missing data by providing fallback values.  
**Example:**  
```sql
SELECT 
  name,
  COALESCE(phone, 'No phone provided') AS contact_number
FROM customers;
```
---

**`NULLIF`**  
Returns `NULL` if two expressions are equal; otherwise, returns the first expression.  
**Why it's handy:** Helps avoid divide-by-zero errors or filter out unwanted matches.  
**Example:**  
```sql
SELECT 
  order_id,
  total_amount,
  quantity,
  total_amount / NULLIF(quantity, 0) AS unit_price
FROM orders;
```
---

**`LEAST`**  
Returns the smallest (minimum) value from a list of expressions.  
**Why it's handy:** Quickly finds the lowest value among multiple columns or expressions.  
**Example:**  
```sql
SELECT 
  name,
  price1,
  price2,
  LEAST(price1, price2) AS lowest_price
FROM products;
```

**`GREATEST`**  
Returns the largest (maximum) value from a list of expressions.  
**Why it's handy:** Useful for comparing columns or fallback strategies to pick the highest value.  
**Example:**  
```sql
SELECT 
  name,
  score1,
  score2,
  GREATEST(score1, score2) AS top_score
FROM test_results;
```
---

**`CAST`**  
Converts a value from one data type to another.  
**Why it's handy:** Ensures proper data type handling for comparisons, calculations, or formatting.  
**Example:**  
```sql
SELECT 
  order_id,
  CAST(order_date AS DATE) AS simple_date,
  CAST(total_amount AS DECIMAL(10,2)) AS amount
FROM orders;
```

---

**`DISTINCT`**  
Returns only unique (non-duplicate) rows from a query result.  
**Why it's handy:** Eliminates duplicates when selecting from columns that may contain repeated values.  
**Example:**  
```sql
SELECT DISTINCT country
FROM customers;
```

---

**`DISTINCT ON`** *(PostgreSQL only)*  
Returns the first row for each unique value of specified columns, based on sort order.  
**Why it's handy:** Efficient way to get "first of each group" without subqueries or window functions.  
**Example:**  
```sql
SELECT DISTINCT ON (customer_id) *
FROM orders
ORDER BY customer_id, order_date DESC;
```
---

**`COUNT()`**  
Returns the number of rows matching a condition or in a group.  
**Why it's handy:** Quickly measures how many records exist in a result set.  
**Example:**  
```sql
SELECT COUNT(*) FROM orders;
```

---

**`MIN()`**  
Returns the smallest value in a group or column.  
**Why it's handy:** Helps identify lowest values—great for dates, prices, etc.  
**Example:**  
```sql
SELECT MIN(price) AS lowest_price FROM products;
```

---

**`MAX()`**  
Returns the largest value in a group or column.  
**Why it's handy:** Useful for finding maximums like latest dates, highest scores, or top prices.  
**Example:**  
```sql
SELECT MAX(score) AS top_score FROM exams;
```

---

**`SUM()`**  
Returns the total of numeric values in a column.  
**Why it's handy:** Perfect for calculating totals, like revenue, quantity, or costs.  
**Example:**  
```sql
SELECT SUM(total_amount) AS total_sales FROM orders;
```

---

**`AVG()`**  
Returns the average (mean) of numeric values in a column.  
**Why it's handy:** Helps measure performance, price trends, and other averages.  
**Example:**  
```sql
SELECT AVG(rating) AS average_rating FROM reviews;
```

---

**`STDDEV()`**  
Returns the statistical standard deviation of a numeric column.  
**Why it's handy:** Measures how spread out values are from the average—great for identifying variability in data.  
**Example:**  
```sql
SELECT STDDEV(score) AS score_std_dev
FROM exam_results;
```

---

**`VAR()`** *(often `VAR_SAMP()` in PostgreSQL)*  
Returns the variance of a numeric column (how spread out the data is).  
**Why it's handy:** Useful for analyzing data distribution and consistency in numerical datasets.  
**Example:**  
```sql
SELECT VAR(score) AS score_variance
FROM exam_results;
```

---

**`REGR_SLOPE()`**  
Returns the slope of the linear regression line for two numeric columns (y over x).  
**Why it's handy:** Great for analyzing trends, like how one variable (e.g. sales) changes in relation to another (e.g. time or price).  
**Example:**  
```sql
SELECT REGR_SLOPE(sales, month) AS sales_trend_slope
FROM monthly_sales;
```

---

**`REGR_INTERCEPT()`**  
Returns the y-intercept of the regression line for two numeric columns.  
**Why it's handy:** Helps you understand the expected starting value (y) when the independent variable (x) is zero—useful in trend forecasting.  
**Example:**  
```sql
SELECT REGR_INTERCEPT(sales, month) AS sales_starting_point
FROM monthly_sales;
```

---

**`CORR()`**  
Calculates the Pearson correlation coefficient between two numeric columns.  
**Why it's handy:** Measures how strongly two variables are related (e.g., `+1` = strong positive, `-1` = strong negative, `0` = no correlation).  
**Example:**  
```sql
SELECT CORR(price, sales) AS price_sales_correlation
FROM product_metrics;
```
