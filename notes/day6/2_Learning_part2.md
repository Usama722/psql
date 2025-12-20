**Day 6 — Continue (Reversal Deep-Dive, Urdu + English Mix)**
(PostgreSQL ERP · SQL-First Learning)

Theek hai. Ab hum **thora slow**, **zyada clear**, aur **Urdu + English mix** ke sath chalain ge — kyun ke ab jo cheez aa rahi hai wo **ERP ka core brain** hai.

Tum already basic samajh chuke ho, ab hum **confusion zero** karte hain.

---

## Step 11 — CRUD Thinking vs ERP Thinking

### Normal Software Soch

> Galat record hai → delete / update kar do

### ERP / Accounting Soch

> Galat record hai → uska **effect neutralize** karo

Yani:

* Data rehne do
* Impact cancel karo

Is ko kehte hain **Reversal Philosophy**

---

## Step 12 — Reversal ko Simple Urdu mein

Socho:
Tum ny ledger mein likh diya:

> Ali ne 10,000 diye

Ab pata chala:

> Ali ne nahi diye

Tum ledger se line **kaat nahi sakte** ❌
Tum neeche likho ge:

> Galti thi → –10,000

Ab ledger bole ga:

```
10,000
-10,000
---------
0
```

Bas. Ye hi reversal hai.

---

## Step 13 — Database Level par Kya Hota Hai

Payment table mein:

* Ek row → original payment
* Doosri row → reversal payment

Donon rows **true** hain
Donon ka record rehna zaroori hai

Is liye:

* DELETE ❌
* UPDATE amount ❌
* INSERT reversal ✔

---

## Step 14 — Important Term: “Posted”

ERP mein har financial entry:

* Draft hoti hai
* Phir **Posted** hoti hai

### Rule:

> **Once posted → immutable**

Posted ke baad:

* Amount change nahi
* Invoice change nahi
* Sirf reversal allowed

---

## Step 15 — Reversal Kab Allowed Hota Hai?

Normally:

* Posted entry
* Within allowed accounting period
* With reason / reference

ERP without reason = fraud risk

---

## Step 16 — Reversal vs Adjustment (Confusion Area)

### Reversal

* Pure cancel
* Full opposite amount

### Adjustment

* Partial correction
* Difference only

Example:

* 10,000 galat post
* Correct amount = 9,500

ERP kare ga:

* Reversal –10,000
* New payment +9,500

Direct –500 ❌ (confusing for audit)

---

## Step 17 — Reporting ka Magic (Important)

Report queries hamesha:

* posted = true
* include both + and – entries

Is liye:

* Reversal hide nahi hota
* Net effect correct hota

Auditor khush ✔
Accountant khush ✔

---

## Step 18 — Tumhara ERP Design Decision

Tumhari system mein:

* Payment table **append-only** honi chahiye
* No physical delete
* No amount update

Sirf:

* status flags
* reversal reference

Ye design **future-proof** hai.

---

## Step 19 — Real ERP Analogy

Socho bank statement:

* Galat entry hui?
* Bank delete nahi karta
* Next line mein correction aati hai

ERP = internal bank

---

## Step 20 — Lock This Sentence (Exam / Interview)

> “ERP systems correct financial mistakes through compensating entries, not by modifying historical data.”

Agar ye sentence samajh aa gaya — **Day 6 jeet liya**.

---

### Next (Day 6 Final Part)

Next hum dekhen ge:

* One table vs two table reversal design
* Flags like `is_reversal`, `reversed_of`
* Report queries jo reversal ko sahi treat karti hain

Ready ho to bolo:
**“Day 6 final continue”**

Agar kisi ek jagah phir bhi halka sa doubt hai, usi point ka naam le lo — us ko aur simplify kar dete hain.
