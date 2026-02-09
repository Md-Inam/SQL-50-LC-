SQL evaluates logical operators in this order:

1️⃣ NOT
2️⃣ AND
3️⃣ OR

---


Suppose
->
WHERE city = 'Delhi' OR city = 'Mumbai' AND age > 25

❌ What SQL actually reads
->
city = 'Delhi'
OR (city = 'Mumbai' AND age > 25)

---
TAKE CONTROL with parentheses
->

WHERE (city = 'Delhi' OR city = 'Mumbai')
AND age > 25


---

Another Example:

Requirement:

Customers who are female and from Delhi or Mumbai

❌ Wrong
WHERE gender = 'Female'
AND city = 'Delhi' OR city = 'Mumbai'

SQL reads:

(gender = 'Female' AND city = 'Delhi')
OR city = 'Mumbai'

So now all Mumbai customers, male included, sneak in.


✅ Correct
WHERE gender = 'Female'
AND (city = 'Delhi' OR city = 'Mumbai')

If a WHERE clause has OR, wrap the OR conditions in ()
Parentheses override precedence



NOT has highest order of precedence

If a condition has OR, group it with parentheses.
If NOT is involved, wrap what you want to negate in parentheses.


---
Example 1: NOT + OR (classic confusion)

Requirement
Customers who are NOT from Delhi or Mumbai

❌ Wrong / risky
WHERE NOT city = 'Delhi' OR city = 'Mumbai'

SQL reads this as:
(NOT city = 'Delhi') OR city = 'Mumbai'

OR because NOT Delhi AND Mumbai means a row must have both Delhi and Mumbai
---

De Morgan’s Law 
This is the rule behind it:
NOT (A OR B) → NOT A AND NOT B
NOT (A AND B) → NOT A OR NOT B


---

Multiple ORs alone cause no ambiguity.
ORs mixed with AND/NOT across decisions cause ambiguity.


---
Case 1: Only ORs → no ambiguity ✅
WHERE name LIKE 'A%'
   OR name LIKE 'S%'
   OR name LIKE 'R%'


---

Case 2: ORs inside a sealed block + AND outside → still no ambiguity ✅

This is what you pointed out:

AND (status != 'inactive' OR status IS NULL)
AND (city IN ('Pune', 'Delhi') OR city IS NULL)
AND (name LIKE 'A%' OR name LIKE 'S%')


Each OR:

is fully contained

cannot “leak” into another condition

is glued together only by AND

So no extra outer () needed.
Each line is already a sealed unit.

---

Case 3: OR competing with AND at the same level → ambiguity ❌
WHERE city = 'Delhi'
OR city = 'Mumbai'
AND age > 25


Here:
OR and AND are at the same level
This must become:

WHERE (city = 'Delhi' OR city = 'Mumbai')
AND age > 25

---

Case 4: OR deciding eligibility + more filters later → ambiguity ❌
WHERE
(city NOT IN ('Chennai','Bangalore') OR city IS NULL)
OR (city = 'Delhi' AND age < 25)
AND status = 'active'


Wrong, because now:

OR decides eligibility

AND adds constraints

precedence ruins intent

Correct:
WHERE
(
  (city NOT IN ('Chennai','Bangalore') OR city IS NULL)
  OR (city = 'Delhi' AND age < 25)
)
AND status = 'active'
---

Are not from Chennai or Bangalore
OR
Are from Delhi but younger than 25

You see “2 different conditions”.
But SQL (and interviews) see this as one gate:
“Does this customer qualify by location/age rule?”

That rule says:
YES if condition A is true
YES if condition B is true
NO otherwise

That’s one eligibility decision with two paths.

---

🧠 LEVEL 1: Correctness parentheses (SQL needs these)
These are mandatory.
Without them, SQL will return wrong rows.

You add these when:
AND and OR appear at the same level
NOT applies to more than one condition

Example:
city = 'Delhi' OR city = 'Mumbai' AND age > 25
❌ Wrong (because AND runs first)

Correct:
(city = 'Delhi' OR city = 'Mumbai')
AND age > 25

👉 This is what you learned first: ambiguity → add ()
This is about correctness.

----
🧠 LEVEL 2: Protection parentheses (SQL doesn’t need these, humans do)

These are optional for SQL, but intentional for safety.

They appear when:

You already fixed ambiguity

But the OR-result is part of a bigger logical decision

And you want to freeze that decision as one unit

Example:

(city NOT IN ('Chennai','Bangalore') OR city IS NULL)
OR (city = 'Delhi' AND age < 25)
AND status = 'active'


This is already ambiguous again, so we first fix it:

(city NOT IN ('Chennai','Bangalore') OR city IS NULL)
OR ((city = 'Delhi' AND age < 25) AND status = 'active')


But that’s not what we want.

What we want is:

First decide eligibility
Then apply status

So we protect the OR-decision:

(
  (city NOT IN ('Chennai','Bangalore') OR city IS NULL)
  OR (city = 'Delhi' AND age < 25)
)
AND status = 'active'


👉 These outer parentheses are not because SQL is confused
👉 They are because we don’t want this OR to mix with future ANDs

This is about intent protection, not precedence.

---
At first, we were fixing wrong answers
Later, we were preventing future wrong answers

---
Not adding Level-2 parentheses is usually NOT “wrong SQL”.
It’s fragile SQL. It works now, breaks later.


---
EX:
WHERE city = 'Delhi'
OR city = 'Mumbai'
AND age > 25
This is already wrong because of precedence.
So you fix it (Level-1 parentheses):

WHERE (city = 'Delhi' OR city = 'Mumbai')
AND age > 25
✅ Correct
✅ SQL is happy
✅ Output is correct
So far So good


Now comes the REAL problem (Level-2)
Tomorrow, you add one more condition:
“Also, status must be active”
You naïvely append it:
WHERE (city = 'Delhi' OR city = 'Mumbai')
AND age > 25
OR status = 'active'
This looks reasonable.
But SQL reads it as:
((city = 'Delhi' OR city = 'Mumbai') AND age > 25)
OR status = 'active'
💥 Result:

ANY active customer is now included

City and age rules are bypassed

No error

Silent data corruption



