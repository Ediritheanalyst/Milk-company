# Milk-company
creating a beginner database to highlight joins on mysql.


## Milk Company Database 🥛

This project demonstrates how to build and analyze a simple Milk Company database using SQL. It covers database creation, table relationships, data insertion, joins, conditional logic, date/time functions, and stored procedures.

## 📂 Database Structure

### Database: milk_company

### 1. milk_employee

Stores employee details.

employeeid INT PRIMARY KEY  
firstname VARCHAR(225)  
lastname VARCHAR(225)  
position VARCHAR(225)

### 2. milk_demographic

Stores employee demographics.

employeeid INT (FK → milk_employee)  
state_of_origin VARCHAR(225)  
marital_status VARCHAR(225)  
residential_address VARCHAR(225)

### 3. milk_sales

Stores sales performance data.

employeeid INT (FK → milk_employee)  
cost_price INT  
sales DOUBLE(15,3)  
profit DECIMAL(15,3)

### 4. milk_order

Tracks order and delivery details.

employeeid INT (FK → milk_employee)  
order_date DATE  
delivery_date DATETIME  
order_time TIME

## 📊 Data Insertion

10 employees with demographics.

10 sales records (cost, sales, profit).

10 orders (order date, delivery date, and time).

🔗 Example Queries & Outputs
1. Employee + Demographic Info
SELECT e.lastname, e.position, d.state_of_origin
FROM milk_employee e
JOIN milk_demographic d
  ON e.employeeid = d.employeeid;

| lastname | position             | state\_of\_origin |
| -------- | -------------------- | ----------------- |
| Doe      | Manager              | Delta             |
| Smith    | Assistant Manager    | Edo               |
| Brown    | Sales Representative | Lagos             |
| Davis    | Marketing Specialist | Ogun              |
| Wilson   | Accountant           | Ondo              |


2. Full Name with Position
SELECT CONCAT(firstname, ' ', lastname) AS full_name, position
FROM milk_employee;

| full\_name    | position             |
| ------------- | -------------------- |
| John Doe      | Manager              |
| Jane Smith    | Assistant Manager    |
| Michael Brown | Sales Representative |
| Emily Davis   | Marketing Specialist |


3. Profit Categorization
SELECT employeeid, profit,
CASE
  WHEN profit >= 33 THEN 'sweet profit'
  WHEN profit >= 29 THEN 'good profit'
  ELSE 'ok profit'
END AS profit_level
FROM milk_sales
ORDER BY profit;

| employeeid | profit | profit\_level |
| ---------- | ------ | ------------- |
| 3          | 27.250 | ok profit     |
| 6          | 29.000 | good profit   |
| 2          | 30.750 | good profit   |
| 8          | 33.500 | sweet profit  |
| 10         | 34.750 | sweet profit  |


4. Order Dates & Times
SELECT employeeid, order_date, 
       DATE(delivery_date) AS delivery_day, 
       TIME(delivery_date) AS delivery_time, 
       order_time
FROM milk_order;

| employeeid | order\_date | delivery\_day | delivery\_time | order\_time |
| ---------- | ----------- | ------------- | -------------- | ----------- |
| 1          | 2024-09-01  | 2024-09-02    | 10:30:00       | 10:00:00    |
| 2          | 2024-09-02  | 2024-09-03    | 11:00:00       | 10:30:00    |
| 3          | 2024-09-03  | 2024-09-04    | 09:45:00       | 09:15:00    |
| 4          | 2024-09-04  | 2024-09-05    | 14:00:00       | 13:30:00    |

⚡ Stored Procedures
Example 1: Profits ≥ 32
CREATE PROCEDURE FUN()
SELECT profit
FROM milk_sales
WHERE profit >= 32;

CALL FUN();

| profit |
| ------ |
| 32.000 |
| 33.500 |
| 34.750 |
| 35.500 |

Example 2: Profits ≥ 32.5 AND Sales ≥ 99
DELIMITER $$
CREATE PROCEDURE jump()
BEGIN
  SELECT profit FROM milk_sales WHERE profit >= 32.50;
  SELECT sales FROM milk_sales WHERE sales >= 99;
END $$
DELIMITER ;

CALL jump();


Profit Output:

| profit |
| ------ |
| 32.000 |
| 33.500 |
| 34.750 |
| 35.500 |

Sales Output:

| sales   |
| ------- |
| 99.500  |
| 102.750 |
| 105.500 |

📌 This project is a beginner-friendly SQL practice database for employee, sales, and order analysis.
