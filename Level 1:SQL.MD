SQL execution precedence:

NOT

AND

OR

---
---




Q1)Basic Filtering

Get all customers from Delhi who are older than 25, sorted by age descending.

  

SELECT * FROM Customers
WHERE city='Delhi' AND age>25
ORDER BY age DESC


--- 

Q2)DISTINCT Reality Check
Find all unique cities where customers are below 30 years old.


SELECT DISTINCT(city) FROM Customers
WHERE age < 30;


---

Q3)  
IN vs OR (Classic)
Get customers who live in Delhi, Mumbai, or Pune.

 
SELECT * FROM Customers 
WHERE city='Delhi' OR city='Mumbai' OR city='Pune';
✅ Correct, but…


⚠️ Interviewers expect IN, not OR spam.
Better (preferred)



SELECT * FROM Customers
WHERE city IN ('Delhi', 'Mumbai', 'Pune');


---

Q4)
BETWEEN Trap
Get all orders placed in March 2024.

SELECT * FROM orders
WHERE order_date BETWEEN '2024-03-01' AND '2024-03-31';



---


Q5) 
NULL Awareness
Find all orders where payment method is missing.

SELECT * FROM orders 
WHERE payment_method IS NULL;


----


Q6) 

NOT Logic
Get all customers NOT from Mumbai.

SELECT customer_id
FROM Customers
WHERE city NOT IN ('Mumbai');



---

Q7)

LIKE (Pattern Thinking)
Find customers whose name starts with ‘A’.


SELECT * FROM customers
WHERE first_name LIKE 'A%';



--- 



Q8)   

Multiple Conditions Together
Find female customers aged 20–30 who do not live in Delhi or Mumbai.

SELECT * FROM Customers
WHERE gender = 'Female'
AND age BETWEEN 20 AND 30
AND city NOT IN ('Delhi', 'Mumbai');




---  



Q9) 

ORDER BY + LIMIT (Meaningful)
Get the top 3 highest-value orders.



SELECT * FROM Orders 
ORDER BY amount DESC
LIMIT 3;
⚠️ Technically correct, analytically weak


SELECT * FROM Orders
WHERE amount IS NOT NULL
ORDER BY amount DESC
LIMIT 3;



---  




Q10)
Business-Sense Filter
Get all products that:
Are in Electronics or Fashion
Have stock available
Cost between 1,000 and 5,000

->
SELECT * FROM products
WHERE category IN ('Electronics', 'Fashion')
AND stock IS NOT NULL
AND stock > 0
AND price BETWEEN 1000 AND 5000;

---
---
🥇 #1 AND / OR Precedence (The Silent Killer)

Question
Find all customers who:
Live in Delhi or Mumbai
Are older than 25

A)
SELECT *
FROM customers
WHERE (city = 'Delhi' OR city = 'Mumbai')
AND age > 25;



---

🥈 #2 NULL + NOT Logic (Interview Favorite)

Question
Find all orders that were NOT paid using Cash.
⚠️ Some orders have payment_method = NULL.

-> WHERE payment_method != 'Cash'
(or)
-> WHERE payment_method NOT LIKE 'Cash'
❌ Both are wrong. This is a classic fail.

Why?
!= 'Cash' excludes NULL rows
NOT LIKE 'Cash' also excludes NULL
The question explicitly said NULLs exist


A)
SELECT *
FROM orders
WHERE payment_method != 'Cash'
OR payment_method IS NULL;



---

🥉 #3 ORDER BY + LIMIT With Meaning

Question
Find the second highest order amount.
⚠️ Duplicates may exist
⚠️ NULL values may exist
⚠️ No JOIN, no GROUP BY

A)
SELECT DISTINCT amount
FROM orders
WHERE amount IS NOT NULL
ORDER BY amount DESC
LIMIT 1 OFFSET 1;



---

---






