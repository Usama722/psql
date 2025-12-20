**Day 5 — Continue (Triggers, Safe Propagation, ERP-Grade Design)**
(PostgreSQL · SQL-First · ERP Learning Roadmap)

Ab hum **soft delete propagation** ko properly design karte hain — bina CASCADE ke, bina data loss ke, aur ERP standards ke mutabiq.

---

## Step 9 — Problem Statement (Real ERP Issue)

Tum chahte ho:

* Invoice delete na ho
* Invoice **inactive** ho jaye
* Us ke **items bhi inactive** ho jain
* Payments **untouched** rahen

Yani:

* Structural data follows invoice
* Financial data remains immutable

---

## Step 10 — Why CASCADE Cannot Help Here

CASCADE sirf:

* `DELETE`
* `UPDATE (PK change)`

Par:

* Soft delete = `UPDATE active = false`
* CASCADE yahan kaam nahi karta

Is liye:
❌ FK CASCADE
✅ Trigger-based propagation

---

## Step 11 — Correct Table Structure (ERP Standard)

### invoices

* id (PK)
* active BOOLEAN DEFAULT TRUE
* deleted_at TIMESTAMP

---

### invoice_items

* id (PK)
* invoice_id (FK)
* active BOOLEAN DEFAULT TRUE

---

### payments

* id (PK)
* invoice_id (FK)
* amount
* posted_at
* **NO active flag deletion logic**

---

## Step 12 — Trigger Concept (High Level)

Rule:

> **Jab invoice inactive ho → items bhi inactive**

But:

* Only once
* Only when active → false
* No effect on payments

---

## Step 13 — Trigger Logic (Human Language)

Trigger checks:

1. Old invoice.active = TRUE
2. New invoice.active = FALSE

If yes:

* Update invoice_items
* Set active = FALSE
* Only for same invoice_id

---

## Step 14 — Why Trigger Is Safe

✔ No DELETE
✔ No CASCADE chain
✔ No accidental payment loss
✔ Full audit trail preserved

This is how:

* SAP
* Oracle Financials
* Odoo

behave internally.

---

## Step 15 — What NOT To Do (Common Mistakes)

❌ Trigger on payments
❌ Trigger on INSERT
❌ Trigger that fires every UPDATE
❌ Trigger without OLD vs NEW check

These cause:

* Performance issues
* Data corruption
* Infinite loops

---

## Step 16 — Reporting Perspective (Very Important)

Reports always filter:

```
WHERE active = true
```

Meaning:

* Old invoices invisible to ops
* Still available for audit
* Historical balances intact

---

## Step 17 — Conceptual Flow (Lock This Image)

1. User “deletes” invoice
2. System:

   * active = false
   * deleted_at = now()
3. Trigger fires:

   * invoice_items → inactive
4. Payments:

   * untouched
   * still linked

No money disappears.

---

## Step 18 — Interview-Level Understanding

If asked:

> “Why not cascade delete payments?”

Correct answer:

> “Payments are financial facts; ERP systems preserve them and apply reversals instead of deletion.”

This answer separates juniors from architects.

---

## Step 19 — Day 5 Core Knowledge (Final Lock)

You now understand:

* DELETE ≠ ERP delete
* CASCADE ≠ safe by default
* Triggers > CASCADE for soft delete
* Payments are immutable records

---

### Next (Day 6 Preview)

We will cover:

* **Reversal entries**
* Credit notes vs refunds
* How ledgers stay balanced
* Why UPDATE on amount is forbidden

When ready, say:
**“Day 6 start”**

Is Day 5 clear now, ya kisi ek part par dubara slow explanation chahiye?
