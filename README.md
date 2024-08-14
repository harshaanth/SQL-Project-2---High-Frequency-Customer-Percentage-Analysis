# Business Question

Write an SQL query to calculate the percentage of high-frequency customers for January 2023. A high-frequency customer is defined as someone who places more than 5 orders in a month. Your output should include only the percentage of these high-frequency customers, rounded to 2 decimal places.

# Thought Process
1. Identifying High-Frequency Customers:

- Define high-frequency customers as those placing more than 5 orders in January 2023.

- Filter out duplicates and null values to ensure accurate counts.

2. Calculating the Ratio:

- Compute the ratio of high-frequency customers to the total number of customers.

- Use DISTINCT to avoid counting the same customer multiple times.

3. Final Calculation:

- Ensure floating-point division by multiplying with 1.00.

- Round the result to two decimal places for precise percentage representation.

# SQL Query

```sql
WITH high_frequency_cust AS (
    SELECT 
        customer_id 
    FROM 
        delivery_order 
    WHERE 
        order_timestamp BETWEEN '2023-01-01' AND '2023-01-31'
        AND order_timestamp IS NOT NULL
        AND customer_id IS NOT NULL
    GROUP BY 
        customer_id 
    HAVING 
        COUNT(*) > 5
)
SELECT 
    ROUND(1.00 * COUNT(DISTINCT hfc.customer_id) / COUNT(DISTINCT d.customer_id) * 100, 2) AS high_frequency_customer_percentage
FROM 
    delivery_order d 
LEFT JOIN 
    high_frequency_cust hfc 
ON 
    hfc.customer_id = d.customer_id 
WHERE 
    d.order_timestamp BETWEEN '2023-01-01' AND '2023-01-31';
