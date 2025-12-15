Perfect! Kal ke liye main aap ke liye **Day 2 PostgreSQL Cheat Sheet** bana deta hoon, **roman urdu** men, step-by-step commands aur exercises ke sath, taake aap 1 ghante men efficiently practice kar saken.

---

# **Day 2: PostgreSQL Cheat Sheet (ERP Tables)**

## **1. PostgreSQL Install & Connect**

* Download: [postgresql.org/download](https://www.postgresql.org/download)
* Install **pgAdmin** ya **psql CLI**
* Superuser password: `postgres`
* Check connection (CLI):

```bash
psql -U postgres -h localhost
```

---

## **2. Database Create karna**

```sql
CREATE DATABASE erp_db;
\c erp_db;   -- connect karna
```

---

## **3. CREATE TABLE Syntax**

```sql
CREATE TABLE table_name (
    column_name data_type constraints,
    column_name2 data_type constraints,
    ...
);
```

**Common Data Types:**

* `SERIAL` → auto-increment ID
* `INT` / `INTEGER`
* `VARCHAR(n)` → short text
* `TEXT` → long text
* `NUMERIC(10,2)` → decimals
* `BOOLEAN` → true/false
* `TIMESTAMP` → date/time

**Constraints:**

* `PRIMARY KEY` → unique ID
* `NOT NULL` → required field
* `UNIQUE` → unique value
* `DEFAULT value` → default value
* `CHECK(condition)` → validation

---

## **4. ERP Tables**

### Customers Table

```sql
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(20),
    active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Products Table

```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    sku VARCHAR(50) UNIQUE NOT NULL,
    price NUMERIC(10,2) NOT NULL DEFAULT 0.00,
    stock INT DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## **5. INSERT Sample Data**

```sql
-- Customers
INSERT INTO customers (name, email, phone)
VALUES
('Ali Khan', 'ali@example.com', '03001234567'),
('Sara Ahmed', 'sara@example.com', '03111234567');

-- Products
INSERT INTO products (name, sku, price, stock)
VALUES
('Wheel', 'WH-001', 500, 20),
('Tyre', 'TY-002', 1200, 10);
```

---

## **6. SELECT Queries**

```sql
-- Sab customers dekho
SELECT * FROM customers;

-- Products jinka stock > 10
SELECT * FROM products WHERE stock > 10;

-- Sirf name aur price dekho
SELECT name, price FROM products;
```

---

## **7. Practice Exercises**

1. Apni marzi ke 3-5 records insert karain har table men.
2. Customer ka phone update karain.
3. Product delete kar ke phir insert karain.
4. Constraints test karain:

   * Duplicate email insert karne ki koshish → fail hona chahiye
   * Negative stock insert karne ki koshish → add `CHECK(stock >= 0)`

```sql
-- Example check constraint add karna
ALTER TABLE products
ADD CONSTRAINT stock_positive CHECK (stock >= 0);
```

---

## **8. Quick ERP Notes**

* ID fields **immutable** → kabhi update na karain
* Defaults aur unique constraints **data integrity** ke liye zaruri
* Next steps: foreign keys, JSONB fields, advanced constraints

---

Kal is cheat sheet ko follow kar ke **1 ghante men Day 2 complete ho jae ga**, aur aap ke paas ready-made ERP tables aur basic queries hon gi.

Agar chaho to main **Day 3 roadmap** bhi abhi bana doon, taake kal ke baad seedha start kar saken.

Kya main Day 3 roadmap bana doon?
