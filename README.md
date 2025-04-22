# Northwind_db
**Practice more SQL here**: [SQL-Practice.com](https://www.sql-practice.com/)
# 📊 Northwind Database SQL Practice
This repository contains a categorized list of SQL questions and answers based on the Northwind database.
The questions are grouped into three difficulty levels: Easy, Medium, and Hard.

---

## 🧠 Difficulty Levels
- ✅ Easy: Basic SELECTs and filters
- ⚙️ Medium: JOINs and aggregations
- 🔥 Hard: Nested queries, complex logic

---

## Easy Questions

### Q1.Show the category_name and description from the categories table sorted by category_name.

```sql
SELECT category_name, description
FROM categories
ORDER BY category_name;
```

### Q2. Show all the contact_name, address, city of all customers which are not from 'Germany', 'Mexico', 'Spain'

```sql
SELECT contact_name, address, city
FROM customers
WHERE Country NOT IN ('Germany','Mexico', 'Spain');
```

### Q3.Show order_date, shipped_date, customer_id, Freight of all orders placed on 2018 Feb 26

```sql
SELECT order_date, shipped_date, customer_id, freight
FROM orders
WHERE order_date = '2018-02-26';
```

### Q4.Show the employee_id, order_id, customer_id, required_date, shipped_date from all orders shipped later than the required date

```sql
SELECT employee_id, order_id, customer_id, required_date, shipped_date
FROM orders
WHERE shipped_date > required_date;
```

### Q5.Show all the even numbered Order_id from the orders table

```sql
SELECT order_id
FROM orders
WHERE mod(order_id,2)=0;
```

### Q6.Show the city, company_name, contact_name of all customers from cities which contains the letter 'L' in the city name, sorted by contact_name

```sql
SELECT city, company_name, contact_name
FROM customers
WHERE city LIKE '%L%'
ORDER BY contact_name ;
```

### Q7.Show the company_name, contact_name, fax number of all customers that has a fax number. (not null)

```sql
SELECT company_name, contact_name, fax
FROM customers
WHERE Fax IS NOT NULL;
```

### Q8.Show the first_name, last_name. hire_date of the most recently hired employee.

```sql
select 
    first_name,
    last_name,
    max(hire_date) as hire_date
  from employees;
```
or

```sql
select 
		first_name, 
		last_name, 
		hire_date 
from employees
where hire_date = (select max(hire_date) from employees);
```

### Q9.Show the average unit price rounded to 2 decimal places, the total units in stock, total discontinued products from the products table.

```sql
SELECT round(avg(Unit_Price), 2) AS average_price,
SUM(units_in_stock) AS total_stock,
SUM(discontinued) as total_discontinued
FROM products;
```


## Medium Questions

### Q1.Show the ProductName, CompanyName, CategoryName from the products, suppliers, and categories table

```sql
SELECT p.product_name, s.company_name, c.category_name
FROM products p
JOIN suppliers s ON s.supplier_id = p.Supplier_id
JOIN categories c On c.category_id = p.Category_id;
```

### Q2.Show the category_name and the average product unit price for each category rounded to 2 decimal places.

```sql
SELECT c.category_name, round(avg(p.unit_price),2) as average_unit_price
FROM products p
JOIN categories c On c.category_id = p.Category_id
GROUP BY c.category_name;
```

### Q3.Show the city, company_name, contact_name from the customers and suppliers table merged together. Create a column which contains 'customers' or 'suppliers' depending on the table it came from.

```sql
select City, company_name, contact_name, 'customers' as relationship 
from customers
union
select city, company_name, contact_name, 'suppliers'
from suppliers;
```

### Q4.Show the total amount of orders for each year/month.

```sql
select 
  year(order_date) as order_year,
  month(order_date) as order_month,
  count(*) as no_of_orders
from orders
group by order_year, order_month;
```


## Hard Questions

### Q1.Show the employee's first_name and last_name, a "num_orders" column with a count of the orders taken, 
-- and a column called "Shipped" that displays "On Time" if the order shipped_date is less or equal to the 
-- required_date, "Late" if the order shipped late, "Not Shipped" if shipped_date is null. Order by employee 
-- last_name, then by first_name, and then descending by number of orders

```sql
SELECT
  e.first_name,
  e.last_name,
  COUNT(o.order_id) As num_orders,
  (
    CASE
      WHEN o.shipped_date <= o.required_date THEN 'On Time'
      WHEN o.shipped_date > o.required_date THEN 'Late'
      WHEN o.shipped_date is null THEN 'Not Shipped'
    END
  ) AS shipped
FROM orders o
  JOIN employees e ON e.employee_id = o.employee_id
GROUP BY
  e.first_name,
  e.last_name,
  shipped
ORDER BY
  e.last_name,
  e.first_name,
  num_orders DESC
  ```
  
### Q2.Show how much money the company lost due to giving discounts each year, order the years from most recent to least recent. Round to 2 decimal places

```sql
Select 
YEAR(o.order_date) AS 'order_year' , 
ROUND(SUM(p.unit_price * od.quantity * od.discount),2) AS 'discount_amount' 

from orders o 
JOIN order_details od ON o.order_id = od.order_id
JOIN products p ON od.product_id = p.product_id

group by YEAR(o.order_date)
order by order_year desc;
```
---
# 📊 Insights & Observations
-🧩 Easy Level Insights
The majority of employees are associated with specific regions and reports-to hierarchies, indicating a well-defined organizational structure.

There is a consistent pattern of orders placed by customers from specific regions, reflecting strong customer retention in certain markets.

Basic joins between tables like orders, employees, and customers demonstrate how relational databases enable a holistic view of transactions and company operations.

-⚙️ Medium Level Insights
Analysis of order details reveals that some products are repeatedly ordered together, which could help in identifying product bundling opportunities.

By calculating the total revenue per order or product, we can identify high-performing items and customers, useful for targeted marketing and inventory decisions.

Medium-difficulty queries involving aggregate functions (e.g., SUM, AVG) help to summarize financial data quickly and support strategic decisions like promotions or bulk discount offerings.

-🚀 Hard Level Insights
Complex joins and subqueries reveal customer behavior trends, such as average order value and top purchasing customers over time.

Queries that analyze supplier contributions help in identifying key suppliers and optimizing the supply chain.

Advanced queries also simulate real-time reporting, such as identifying the top 5 products or the most profitable months — essential for executive dashboards and BI tools.
