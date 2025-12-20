**Day 5 — Proper Start (Step-by-Step, ERP Mindset)**
(PostgreSQL · SQL-First · ERP Learning Roadmap)

Aaj hum **DELETE philosophy** ko foundation se samjhenge. Ye wo jagah hai jahan 90% ERP systems ghalat ban jate hain.

---

## Step 1 — ERP Golden Rule (Lock This)

> **ERP mein DELETE almost kabhi nahi hota**

Reason:

* ERP = history
* History = audit
* Audit = no data loss

Is liye ERP systems mein:

* `DELETE` ❌ (rare)
* `UPDATE active = false` ✅ (standard)

---

## Step 2 — Hard Delete vs Soft Delete

### 2.1 Hard Delete

```
DELETE FROM invoices WHERE id = 10;
```

Kya hota hai?

* Record database se gaib
* Foreign keys trigger
* CASCADE lag gaya → chain reaction

ERP mein:
❌ Dangerous
❌ Audit break
❌ Reports distort

---

### 2.2 Soft Delete (ERP Standard)

Table design:

```
active BOOLEAN DEFAULT TRUE
deleted_at TIMESTAMP NULL
```

Delete ka matlab:

```
active = false
deleted_at = now()
```

✔ Data safe
✔ Reports stable
✔ Legal compliance

---

## Step 3 — Apply This to Your Tables

### invoices

* ❌ Hard delete
* ✅ Soft delete only

Reason:
Invoice is a **legal document**

---

### invoice_items

Two options (ERP style):

#### Option A (Strict)

* Invoice inactive → items auto inactive (via trigger)

#### Option B (Simple)

* Items remain, fetched only via active invoice

---

### payments

* ❌ Hard delete
* ❌ CASCADE
* ✅ Never delete
* Only:

  * Reverse payment
  * Adjustment entry

Payments are **facts**, not editable objects.

---

## Step 4 — Real ERP Payment Reversal (Concept)

Instead of:

```
DELETE FROM payments WHERE id = 5;
```

ERP does:

* New entry:

  * negative amount
  * reference to original payment

Ledger remains balanced.

---

## Step 5 — Foreign Key Strategy (Final Form)

### invoice_items FK

```
FOREIGN KEY (invoice_id)
REFERENCES invoices(id)
ON DELETE CASCADE
```

Used **only** if you still allow hard delete for invoices
(learning phase)

---

### payments FK

```
FOREIGN KEY (invoice_id)
REFERENCES invoices(id)
ON DELETE RESTRICT
```

Means:

* Invoice delete blocked if payment exists

---

## Step 6 — Why RESTRICT is Your Friend

RESTRICT:

* Forces you to **think**
* Prevents accidental loss
* Matches accounting rules

Error message = design guardrail

---

## Step 7 — Mental Check Before Any DELETE

Ask yourself:

1. Is this data ever used in reports?
2. Can auditor ask for this record?
3. Does money touch this table?

If **YES** to any → ❌ DELETE

---

## Step 8 — Day 5 Locked Knowledge

By now you should clearly know:

* CASCADE ≠ always good
* Child ≠ always deletable
* Payments ≠ normal child
* ERP prefers **state change over removal**

---

### Next (Day 5 Continue)

We will cover:

* **Triggers for soft delete propagation**
* How invoice inactive → items inactive automatically
* Safe design pattern used in SAP / Oracle ERP

When ready, say:
**“Day 5 continue”**
