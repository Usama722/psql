**Day 6 — Start (Reversals, Credit Notes, and Ledger Discipline)**
(PostgreSQL ERP Learning · SQL-First Roadmap)

Good. Tumhara “mind atakna” bilkul **valid** tha — reversal ERP ka sab se critical aur least-understood concept hota hai. Aaj hum isko **zero ambiguity** ke sath lock karenge.

---

## Day 6 Objective

Aaj ke baad tum:

1. Samjho ge **reversal kya hota hai** (technical + accounting)
2. Jaano ge **delete / update kyun forbidden** hota hai
3. Distinguish karo ge:

   * payment reversal
   * credit note
   * refund
4. Ledger ka balance **kabhi break nahi hoga**

---

## Step 1 — ERP Golden Rule #2

> **Financial records are immutable**

Immutable ka matlab:

* Once posted → **never changed**
* Na amount update
* Na delete
* Na overwrite

Sirf **new entries** allowed hoti hain.

---

## Step 2 — Simple Definition (Lock This)

### Reversal kya hota hai?

> **Original transaction ko cancel karne ke liye equal and opposite transaction create karna**

Delete ❌
Update ❌
Reverse entry ✅

---

## Step 3 — Real Life Example (Very Simple)

Customer ne:

* 10,000 receive kiye
* Galti se wrong invoice par post ho gaya

### Galat approach ❌

* Payment delete
* Amount update

### ERP approach ✅

1. Original payment stays
2. New payment entry:

   * amount = **–10,000**
   * reference = original payment
   * remark = “Reversal due to wrong invoice”

Net effect:

```
10,000 + (-10,000) = 0
```

Ledger clean ✔
Audit intact ✔

---

## Step 4 — Why ERP Never Deletes Payments

Socho agar delete allow hota:

* Bank statement ≠ system
* Audit fail
* Fraud easy
* History lost

Is liye:

> **ERP systems trust math, not deletion**

---

## Step 5 — Reversal vs Refund (Critical Difference)

### Reversal

* Mistake correction
* Same accounting period (usually)
* No cash movement (logical cancel)

### Refund

* Real money goes back to customer
* New cash movement
* Bank entry required

Refund ≠ reversal
Refund may include reversal + new payment out

---

## Step 6 — Credit Note vs Reversal

### Credit Note

* Invoice side correction
* Reduces receivable
* Linked to invoice

### Payment Reversal

* Payment side correction
* Neutralizes cash impact

Invoice galat → Credit Note
Payment galat → Reversal

---

## Step 7 — Database Design Implication (High Level)

Payment table should support:

* negative amounts
* reference to original payment
* reversal flag or type

But:

* No DELETE
* No UPDATE on amount

---

## Step 8 — Common ERP Flow (End-to-End)

1. Invoice created
2. Payment posted
3. Mistake found
4. System creates:

   * reversal entry
5. Correct payment posted again

History preserved. Balance correct.

---

## Step 9 — Why Your Mind Got Stuck (Truth)

Because normal CRUD thinking says:

> “Galat hai → delete karo”

ERP thinking says:

> “Galat hai → compensate karo”

Ye mindset shift **Day 6 ka core** hai.

---

## Step 10 — Interview Lock Statement

If someone asks:

> “Why not update payment amount?”

Correct answer:

> “Because financial transactions must be immutable; corrections are done via reversal entries to preserve audit trails.”

---

## Day 6 Summary (Lock It)

* Reversal = opposite transaction
* Delete = forbidden
* Update = forbidden
* Ledger must always reconcile
* History > convenience

---

### Next (Day 6 Continue)

We will go deeper into:

* **Schema patterns for reversals**
* One-table vs two-table approach
* Posting flags (`draft`, `posted`, `reversed`)
* How reports ignore reversed entries safely

When ready, say:
**“Day 6 continue”**
