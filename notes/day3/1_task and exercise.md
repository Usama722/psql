Theek hai. **Day 3** shuru karte hain — SQL-first ERP roadmap ke mutabiq, practical aur production-oriented focus ke sath.

---

## **Day 3: Relational Design, Constraints & Indexing (ERP Core)**

### **Day 3 Objective**

* Tables ke darmiyan **proper relationships** banana
* **Data integrity** enforce karna (DB level par)
* **Performance basics** samajhna (indexes)
* ERP-style thinking develop karna: *data galat insert hi na ho*

---

## **1. Foreign Keys (Relations)**

### **Concept**

ERP mein tables isolated nahi hoti:

* Customers → Orders
* Products → Order Items
* Companies → Branches

Foreign key ensure karti hai:

* Parent record exist karta ho
* Orphan data na bane

### **Example**

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATE DEFAULT CURRENT_DATE,

    CONSTRAINT fk_customer
        FOREIGN KEY (customer_id)
        REFERENCES customers(id)
);
```

**Important Points**

* `customers.id` pehle exist hona chahiye
* Galat `customer_id` insert hi nahi hoga

---

## **2. ON DELETE / ON UPDATE Rules**

### **Why important in ERP**

Socho:

* Customer delete hua → Orders ka kya hoga?

### **Common Rules**

```sql
ON DELETE RESTRICT   -- delete rok do
ON DELETE CASCADE    -- child records auto delete
ON DELETE SET NULL   -- relation hata do
```

### **ERP Safe Default**

```sql
ON DELETE RESTRICT
```

Example:

```sql
FOREIGN KEY (customer_id)
REFERENCES customers(id)
ON DELETE RESTRICT
```

---

## **3. CHECK Constraints (Business Rules)**

### **Example: Quantity & Price**

```sql
qty > 0
price >= 0
```

```sql
CREATE TABLE order_items (
    id SERIAL PRIMARY KEY,
    order_id INT,
    product_id INT,
    qty INT CHECK (qty > 0),
    price NUMERIC(10,2) CHECK (price >= 0)
);
```

**Rule of Thumb**

> Jo cheez kabhi valid nahi ho sakti, usko DB hi reject kare.

---

## **4. UNIQUE Constraints (Duplicate Control)**

ERP mein duplicates bohot dangerous hotay hain.

### **Examples**

* Customer name
* Product SKU
* Invoice number

```sql
sku VARCHAR(50) UNIQUE
```

Or composite unique:

```sql
UNIQUE (order_id, product_id)
```

---

## **5. Indexes (Performance Foundation)**

### **Why Index?**

* ERP mein search hoti hai:

  * Customer by name
  * Product by SKU
  * Orders by date

### **Create Index**

```sql
CREATE INDEX idx_customers_name
ON customers(name);
```

### **Golden Rule**

* WHERE
* JOIN
* ORDER BY
  → in columns par index lagta hai

---

## **6. ERP Thinking (Very Important)**

Aaj ka sab se important mindset shift:

| Galat Approach           | Sahi ERP Approach            |
| ------------------------ | ---------------------------- |
| Validation sirf frontend | Validation **DB + API + UI** |
| Jo data aa jaye insert   | Rules pehle define           |
| Later fix                | Prevent at source            |

---

## **Day 3 Practice Task (Must Do)**

1. `orders` table banao

   * FK → customers
2. `order_items` table banao

   * FK → orders
   * FK → products
3. `qty > 0` constraint
4. `(order_id, product_id)` UNIQUE
5. Index on:

   * `customers(name)`
   * `products(sku)`

---

## **Next Day (Day 4 Preview)**

* Transactions (`BEGIN / COMMIT / ROLLBACK`)
* Real-life ERP failure scenarios
* Power cut / API crash handling (tumhara pehle ka sawal 👌)

Jab ready ho, bolo:
**“Day 3 practice check”**
phir main tumhara schema review karunga, production-level feedback ke sath.
