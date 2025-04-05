
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

---

**Windowing (Window Functions)**  
Windowing refers to performing calculations across a "window" (subset) of rows related to the current row—without collapsing the result set.  
**Why it's handy:** Lets you do things like running totals, rankings, moving averages, and comparisons *within groups* of rows, while still keeping all rows in the output.  
**Example:**  
```sql
SELECT 
  customer_id,
  order_id,
  order_date,
  SUM(total_amount) OVER (PARTITION BY customer_id ORDER BY order_date) AS running_total
FROM orders;
```

---

**`PARTITION BY`**  
Divides the result set into groups (partitions) for window functions to operate within.  
**Why it's handy:** Enables row-level calculations (like ranking or running totals) scoped to logical groups—without collapsing rows like `GROUP BY` would.  
**Example:**  
```sql
SELECT 
  customer_id,
  order_id,
  order_date,
  ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date DESC) AS row_num
FROM orders;
```

---

**`ROW_NUMBER()`**  
Assigns a unique, sequential number to each row within a result set, based on a specified ordering.  
**Why it's handy:** Great for pagination, deduplication, or selecting the "first" row in a group.  
**Example:**  
```sql
SELECT 
  customer_id,
  order_id,
  order_date,
  ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY order_date DESC) AS row_num
FROM orders;
```

---

**`RANK()`**  
Assigns a rank to each row within a partition, with gaps for tied values.  
**Why it's handy:** Useful when ranking data with equal values—tied items get the same rank, and the next rank is skipped accordingly.  
**Example:**  
```sql
SELECT 
  student_id,
  test_score,
  RANK() OVER (ORDER BY test_score DESC) AS rank
FROM test_results;
```

---

**`DENSE_RANK()`**  
Assigns a rank to each row, like `RANK()`, but **without gaps** between tied values.  
**Why it's handy:** Useful when you want ties to share a rank but keep the next rank consecutive—great for clean, gapless rankings.  
**Example:**  
```sql
SELECT 
  student_id,
  test_score,
  DENSE_RANK() OVER (ORDER BY test_score DESC) AS dense_rank
FROM test_results;
```

---

**`NTILE()`**  
Distributes rows into a specified number of roughly equal-sized buckets (tiles).  
**Why it's handy:** Great for percentile-based analysis—like quartiles, deciles, or custom groupings.  
**Example:**  
```sql
SELECT 
  customer_id,
  total_spent,
  NTILE(4) OVER (ORDER BY total_spent DESC) AS spending_quartile
FROM customers;
```

---

**`LAG()`**  
Accesses a value from a previous row in the same result set (based on a defined order).  
**Why it's handy:** Great for calculating differences between rows, tracking changes over time, or detecting trends.  
**Example:**  
```sql
SELECT 
  employee_id,
  salary,
  LAG(salary) OVER (PARTITION BY department_id ORDER BY hire_date) AS previous_salary
FROM employees;
```

---

**`LEAD()`**  
Accesses a value from a following row in the same result set (based on a defined order).  
**Why it's handy:** Great for comparing current values with future ones—helps identify upcoming changes or trends.  
**Example:**  
```sql
SELECT 
  employee_id,
  salary,
  LEAD(salary) OVER (PARTITION BY department_id ORDER BY hire_date) AS next_salary
FROM employees;
```

---

**`STR_TO_DATE()`** *(MySQL-specific)*  
Converts a string into a date using a specified format.  
**Why it's handy:** Essential for importing, parsing, or comparing date strings in custom formats.  
**Example:**  
```sql
SELECT STR_TO_DATE('04-04-2025', '%d-%m-%Y') AS parsed_date;
```

---

**`EXTRACT()`**  
Pulls out specific parts of a date or timestamp (like year, month, day, etc.).  
**Why it's handy:** Useful for grouping, filtering, or analyzing data based on time components.  
**Example:**  
```sql
SELECT 
  order_id,
  order_date,
  EXTRACT(YEAR FROM order_date) AS order_year
FROM orders;
```

---

**`DATE_ADD()`** *(MySQL-specific)*  
Adds a time interval to a date value.  
**Why it's handy:** Perfect for calculating future dates, setting expiration periods, or scheduling events.  
**Example:**  
```sql
SELECT 
  order_date,
  DATE_ADD(order_date, INTERVAL 7 DAY) AS delivery_date
FROM orders;
```

---

**`DATE_SUB()`** *(MySQL-specific)*  
Subtracts a time interval from a date value.  
**Why it's handy:** Great for filtering or calculating past dates—like finding data from "7 days ago" or setting expiration windows.  
**Example:**  
```sql
SELECT 
  CURRENT_DATE AS today,
  DATE_SUB(CURRENT_DATE, INTERVAL 30 DAY) AS thirty_days_ago;
```

---

**`INTERVAL`**  
Represents a span of time used in date arithmetic (e.g., days, months, years).  
**Why it's handy:** Lets you add or subtract flexible time values in a readable way across many SQL functions.  
**Example (MySQL):**  
```sql
SELECT 
  order_date,
  DATE_ADD(order_date, INTERVAL 1 MONTH) AS next_billing_date
FROM subscriptions;
```

---

**`DATEDIFF()`** *(MySQL & SQL Server)*  
Returns the number of days between two dates.  
**Why it's handy:** Simple way to calculate durations—like age, overdue days, or time between events.  
**Example (MySQL/SQL Server):**  
```sql
SELECT 
  DATEDIFF(CURRENT_DATE, order_date) AS days_since_order
FROM orders;
```

---

**`TIMESTAMPDIFF()`** *(MySQL-specific)*  
Returns the difference between two date/timestamp values in the unit you specify.  
**Why it's handy:** Lets you calculate exact differences in days, months, hours, minutes, etc.—not just days like `DATEDIFF()`.  
**Example:**  
```sql
SELECT 
  TIMESTAMPDIFF(DAY, order_date, CURRENT_DATE) AS days_elapsed,
  TIMESTAMPDIFF(MONTH, signup_date, CURRENT_DATE) AS account_age_months
FROM users;
```
