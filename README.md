# High-Frequency-Customer-Percentage-Analysis
This project aims to calculate the percentage of high-frequency customers for January 2023 using SQL. A high-frequency customer is defined as someone who places more than 5 orders within a month. The output is rounded to two decimal points to present a precise percentage of these high-frequency customers.

# SQL Query Explanation
The SQL query is structured as follows:

Identifying High-Frequency Customers:

We use a Common Table Expression (CTE) to identify customers who placed more than 5 orders in January 2023.
The CTE filters out null values and groups the orders by customer ID, ensuring only high-frequency customers are included.

Calculating the Ratio:

The main query calculates the ratio of high-frequency customers to the total number of distinct customers who placed any orders in the same period.
The ratio is then multiplied by 1.00 to ensure floating-point division and rounded to two decimal points for accuracy.
