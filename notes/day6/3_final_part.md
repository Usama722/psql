**Day 6 — Draft ka Confusion (Clear + Final Explanation)**
(Urdu + English mix, bilkul seedha jawab)

Tumhara sawal **bohat valid** hai, aur yahan par aksar log confuse hotay hain. Chalo isko **clean tareeqay se separate** kar dete hain.

---

## Short Answer (Pehlay lock kar lo)

**Draft do qisam ka hota hai:**

1. **UI Draft** → database ka hissa hi nahi
2. **DB Draft (Accounting Draft)** → database mein hota hai, but not posted

Ye **do different cheezen** hain. Ab detail.

---

## 1️⃣ UI Draft (Front-End Draft)

### Matlab:

* User form bhar raha hai
* Save / Submit abhi nahi kiya
* Data sirf:

  * browser memory
  * React state
  * temporary JSON

### Important:

* ❌ Database mein koi row nahi
* ❌ ERP / accounting ka koi concern nahi

Is stage par:

* user cancel kare → kuch bhi nahi hota
* power off ho jaye → koi issue nahi

👉 **Isko hum “draft” nahi mante ERP terms mein**

Ye sirf **UI state** hai.

---

## 2️⃣ DB Draft (Accounting Draft) ✅

### Ye real ERP draft hota hai

Example:

* Invoice create hui
* Save ho gai
* But **post nahi hui**

Table mein row exist karti hai:

* id assign ho chuka
* amounts saved
* editable hai

Yahan **draft column / status hota hai**

---

## 3️⃣ Proper ERP Column Design

Normally ERP systems use:

* `status` column
  Values:

  * `draft`
  * `posted`
  * `reversed`
  * `cancelled`

Ya phir:

* `posted BOOLEAN`

---

## 4️⃣ Difference Clear Kar Lo (Very Important)

| Cheez             | UI Draft | DB Draft         |
| ----------------- | -------- | ---------------- |
| Database row      | ❌ No     | ✅ Yes            |
| Editable          | Yes      | Yes              |
| Accounting effect | ❌ No     | ❌ No             |
| Ledger impact     | ❌ No     | ❌ No             |
| Can be deleted    | Yes      | Yes (only draft) |

---

## 5️⃣ Posting Moment (Most Critical)

Jab:

* user “Post” button press kare
* system bole:

  > **Now this becomes accounting truth**

Tab:

* status → `posted`
* row becomes immutable
* reversal rules activate

---

## 6️⃣ Final ERP Rule (Lock This)

> **Draft ka matlab hai: database mein hai, lekin accounting mein nahi**

UI wala draft = ERP ke liye **exist hi nahi karta**

---

## 7️⃣ Tumhari Learning ke liye Best Model

Tum SQL-First seekh rahe ho, is liye:

* UI draft → ignore
* DB draft → samjho + design karo
* Posted → immutable
* Reversal → compensation entry

Ye flow bilkul **SAP / Oracle level** hai.

---

## Day 6 — Final Lock Summary

* Draft column **DB draft ke liye hota hai**
* UI draft = sirf front-end concern
* Posted ke baad:

  * no delete
  * no update
  * only reversal
* ERP history kabhi overwrite nahi karta

---

Agar ab ye clear ho gaya hai to confidently bolo:
**“Day 6 complete”**

Us ke baad hum **Day 7 (Ledger & Double Entry Concept)** start kar sakte hain — jo ERP ka next backbone hai.
