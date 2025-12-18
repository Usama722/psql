Theek hai — **Day 4 continue** karte hain.

---

## **Day 4 (Part 2): ON UPDATE CASCADE, Composite FKs, Soft vs Hard Delete**

Yahan se hum **ERP-grade design thinking** mein enter karte hain.

---

## **1. ON UPDATE CASCADE — Kab Zaroori, Kab Nahi**

### Concept

`ON UPDATE CASCADE` ka matlab:

* Parent table ki **PK change ho**
* Child table ki FK **automatically update ho jaye**

```sql
FOREIGN KEY (customer_id)
REFERENCES customers(id)
ON UPDATE CASCADE
```

### Reality Check (ERP Rule)

> **Primary Keys should NEVER change**

Is liye:

* `SERIAL / ID` based PKs → **ON UPDATE CASCADE useless**
* Natural keys (code, sku, invoice_no) → **sometimes needed**

### Example (Valid Use Case)

```sql
products (
    id SERIAL PK,
    sku VARCHAR UNIQUE
)
order_items (
    product_sku VARCHAR FK → products.sku
)
```

Yahan agar SKU change ho:

```sql
ON UPDATE CASCADE
```

✔ Makes sense

---

## **2. Composite Foreign Keys (Advanced but Real ERP)**

### Kya hota hai?

Foreign key jo **multiple columns** par based ho.

### Example Scenario

* Company-wise data isolation
* Same customer ID different companies mein ho sakta hai

#### Parent table

```sql
customers (
    company_id INT,
    customer_id INT,
    name TEXT,
    PRIMARY KEY (company_id, customer_id)
)
```

#### Child table

```sql
orders (
    company_id INT,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (company_id, customer_id)
        REFERENCES customers(company_id, customer_id)
)
```

### ERP Benefit

* Cross-company data leak impossible
* Multi-tenant system ready

---

## **3. Soft Delete vs Hard Delete (VERY IMPORTANT)**

### Hard Delete

```sql
DELETE FROM customers WHERE id = 5;
```

❌ Problems:

* History lost
* Audit trail broken
* Accounting issues

### Soft Delete (ERP Standard)

```sql
active BOOLEAN DEFAULT TRUE
```

Instead of delete:

```sql
UPDATE customers SET active = FALSE WHERE id = 5;
```

✔ Data safe
✔ Reports intact
✔ Legal compliance

---

## **4. CASCADE + Soft Delete — Golden Rule**

> **Never mix CASCADE with soft delete logic**

Why?

* CASCADE works only on **DELETE**
* Soft delete uses **UPDATE**

Correct approach:

* Master tables → Soft delete + RESTRICT
* Transaction tables → Hard delete + CASCADE

---

## **5. Referential Integrity ka Golden Matrix**

| Table Type                   | Delete Type | FK Rule  |
| ---------------------------- | ----------- | -------- |
| Master (customers, products) | Soft        | RESTRICT |
| Transactions (orders)        | Hard        | RESTRICT |
| Details (order_items)        | Hard        | CASCADE  |
| Logs / temp                  | Hard        | CASCADE  |

---

## **6. Common Beginner Mistakes (Avoid These)**

❌ CASCADE everywhere
❌ Updating primary keys
❌ Deleting master data
❌ FK na lagana “performance ke liye”

ERP systems **constraints se strong hotay hain**, weak nahi.

---

## **7. Mini Design Exercise (Think First)**

Tables:

* invoices
* invoice_items
* payments

Socho:

* Kaun parent?
* Kaun child?
* Kahan CASCADE?
* Kahan soft delete?

(Answer kal khud btao gay 😉)

---

## **Day 4 Complete — You Have Leveled Up**

Ab aap:

* FK sirf syntax nahi, **design tool** ki tarah use kar rahe ho
* ERP-safe database decisions lena shuru kar chukay ho

---

### **Next: Day 5 Preview**

* CHECK constraints
* UNIQUE (partial unique)
* ENUM vs lookup tables
* Data validation at DB level

Jab ready ho, likho:
**“Day 5 start”**
