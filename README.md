# 🍕 Zomato Sales Analysis
> A structured SQL project analysing a Zomato-style food delivery database to uncover insights on customer behaviour, restaurant performance, delivery efficiency, and revenue trends.

![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-Analysis-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)

---

## 📌 Project Overview

This project involves analysing a Zomato sales database containing real-world style data on customers, orders, restaurants, food items, employees, and payments. The goal was to write structured SQL queries to extract meaningful business insights — simulating the kind of analysis a Data Analyst would perform for a food delivery platform.

**Tool Used:** MySQL Workbench  
**Database:** `zomatodf`  
**Total Queries Written:** 15  

---

## 🗃️ Database Schema

The database consists of **7 tables** with the following structure:

| Table | Rows | Key Columns |
|-------|------|-------------|
| `customer` | 17 | customer_id, customer_name, contact_number, address |
| `foods` | 12 | food_id, food_name, price_per_unit |
| `order_detail` | 43 | order_id, customer_id, restaurant_id, employee_id, order_status, order_time, delivered_time |
| `order_food` | 61 | orderF_id, order_id, customer_id, restaurant_id, food_id, quantity, employee_id |
| `payment_table` | 43 | transaction_id, order_id, payment_type, payment_status |
| `restaurant` | 7 | restaurant_id, restaurant_name, rlocation, rrating |
| `zomato_employee` | 15 | employee_id, employee_name, emp_contact_number, employee_avg_rating |

### Entity Relationship Diagram
- `order_detail` is the central fact table linked to customer, restaurant, and employee
- `order_food` bridges orders with food items and captures quantity
- `payment_table` links to orders for payment analysis
- `foods`, `restaurant`, `zomato_employee`, `customer` are dimension tables

---

## 🔍 Business Problems Solved

### 👥 Customer Analysis
**Q1. Top 3 customers by number of orders**
- Used `JOIN` between customer and order_detail + `COUNT` + `GROUP BY` + `ORDER BY` + `LIMIT`
- *Insight: Identified the most loyal/frequent customers on the platform*

**Q9. Average order amount per customer**
- Multi-table JOIN across customer, order_food, foods + `AVG` + `ROUND`
- *Insight: Ranked customers by spending — helps identify high-value customers*

**Q10. Most frequent customer per restaurant**
- 3-table JOIN + `COUNT` of orders per customer per restaurant
- *Insight: Reveals which customers drive repeat business for each restaurant*

---

### 🍽️ Restaurant Performance
**Q2. Restaurant with highest average rating**
- `AVG` on rrating + `GROUP BY` + `ORDER BY DESC` + `LIMIT 1`
- *Insight: Pinpoints the top-rated restaurant for quality benchmarking*

**Q5. Second highest revenue-generating restaurant**
- 3-table JOIN + `SUM` of price_per_unit + `OFFSET 1` with `LIMIT 1`
- *Insight: Identifies runner-up restaurant — useful for competitive analysis*

**Q14. Restaurant with most diverse menu**
- JOIN order_food with foods + `COUNT(food_id)` + `GROUP BY restaurant_id`
- *Insight: Finds which restaurant offers the widest variety to customers*

---

### 🍔 Food Item Analysis
**Q4. Total revenue by each food item**
- JOIN foods with order_food + `SUM(price_per_unit)` + ordered by revenue descending
- *Insight: Reveals top revenue-generating food items for menu optimization*

**Q6. Top 5 most popular food items by quantity sold**
- JOIN order_food with foods + `SUM(quantity)` + `LIMIT 5`
- *Insight: Identifies best-selling items — useful for inventory and promotions*

**Q13. Top 5 most expensive food items**
- Simple `ORDER BY price_per_unit DESC` + `LIMIT 5`
- *Insight: Highlights premium items for pricing strategy analysis*

---

### 🚚 Delivery & Operations Analysis
**Q3. Orders delivered in under 30 minutes**
- `TIMESTAMPDIFF(MINUTE, order_time, delivered_time)` with `WHERE < 30`
- *Insight: Measures fast deliveries — key operational KPI for delivery efficiency*

**Q7. Top 3 employees by average delivery rating**
- `AVG(employee_avg_rating)` + `GROUP BY` + `ORDER BY DESC` + `LIMIT 3`
- *Insight: Identifies best-performing delivery employees for rewards/recognition*

**Q11. Total orders placed on weekends**
- `DAYOFWEEK()` function with `IN (1,7)` — Sunday=1, Saturday=7
- *Insight: Measures weekend demand — helps with staffing and resource planning*

**Q12. Average delivery time — Weekdays vs Weekends**
- `CASE WHEN DAYOFWEEK() IN (1,7) THEN 'Weekend' ELSE 'Weekday'` + `AVG(TIMESTAMPDIFF)`
- *Insight: Compares delivery performance across day types — operational planning*

---

### 📅 Time-Based Analysis
**Q8. Month with highest number of orders**
- `DATE_FORMAT(order_time, '%Y-%m')` + `COUNT(*)` + `GROUP BY` + `LIMIT 1`
- *Insight: Identifies peak order month — useful for seasonal planning*

---

### 💳 Payment Analysis
**Q15. Total payment amount by payment type**
- JOIN payment_table with order_food and foods + `SUM` + `GROUP BY payment_type`
- *Insight: Reveals preferred payment methods — helps optimize checkout experience*

---

## 🛠️ SQL Concepts Used

| Concept | Used In |
|---------|---------|
| `JOIN` (INNER) | Q1, Q4, Q5, Q6, Q9, Q10, Q14, Q15 |
| `GROUP BY` + `ORDER BY` | All aggregation queries |
| `COUNT`, `SUM`, `AVG`, `ROUND` | Q1, Q2, Q4, Q6, Q7, Q9, Q11, Q12 |
| `LIMIT` + `OFFSET` | Q2, Q5, Q6, Q13 |
| `TIMESTAMPDIFF()` | Q3, Q12 |
| `DAYOFWEEK()` | Q11, Q12 |
| `DATE_FORMAT()` | Q8 |
| `CASE WHEN` | Q12 |
| Subquery / Multi-table JOIN | Q5, Q9, Q10, Q15 |

---

## 💡 Key Business Insights

- 🏆 **Top customers** drive a disproportionate share of orders — loyalty program opportunity
- ⭐ **Highest-rated restaurant** can be used as a benchmark for quality standards
- 🍔 **Top 5 food items** by quantity reveal which dishes dominate sales volume
- 💰 **Second highest revenue restaurant** shows a competitive gap worth analysing
- 🚀 **Fast deliveries (<30 min)** can be highlighted as a platform quality metric
- 📅 **Peak order month** identified — enables seasonal staffing and marketing planning
- 🗓️ **Weekend vs Weekday delivery time** comparison surfaces operational inefficiencies
- 💳 **Payment type breakdown** shows customer preferences for digital vs cash payments

---

## 📂 Files in This Repository

| File | Description |
|------|-------------|
| `zomato_queries.sql` | All 15 SQL queries with comments |
| `zomatodf.sql` | Database creation and data insertion script |
| `README.md` | Project documentation |

---

## 🚀 How to Run

1. Open **MySQL Workbench**
2. Run `zomatodf.sql` to create and populate the database
3. Open `zomato_queries.sql`
4. Execute queries one by one or all at once
5. View results in the output panel

---

## 👤 Author

**Ansh Goyal**  
📧 anshgoyal1205@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/ansh-goyal-180b19212)  
🐙 [GitHub](https://github.com/anshgoyal1205)
