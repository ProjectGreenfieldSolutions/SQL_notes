
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
