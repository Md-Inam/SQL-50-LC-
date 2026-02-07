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

