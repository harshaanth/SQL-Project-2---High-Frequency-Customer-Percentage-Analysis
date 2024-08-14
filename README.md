# Business Question?
Write an SQL query to calculate the percentage of high-frequency customers for January 2023. A high-frequency customer is defined as someone who places more than 5 orders in a month. Your output should include only the percentage of these high-frequency customers, rounded to 2 decimal places.

# SQL Query

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

# SQL Query Explanation

The SQL query is structured as follows:

1. Identifying High-Frequency Customers:

- We use a Common Table Expression (CTE) to identify customers who placed more than 5 orders in January 2023.
  
- The CTE filters out null values and groups the orders by customer ID, ensuring only high-frequency customers are included.

2. Calculating the Ratio:

- The main query calculates the ratio of high-frequency customers to the total number of distinct customers who placed any orders in the same period.
  
- The ratio is then multiplied by 1.00 to ensure floating-point division and rounded to two decimal points for accuracy.

# Order of Execution:

1. Common Table Expression (CTE) (high_frequency_cust):

- The subquery within the CTE is executed:
  
  FROM delivery_order: The delivery_order table is selected as the initial dataset.
  
  WHERE clause: Filters rows where order_timestamp is between '2023-01-01' and '2023-01-31', and ensures order_timestamp and customer_id are not null.

  GROUP BY clause: Groups the filtered rows by customer_id.

  HAVING clause: Filters the groups to include only those with more than 5 orders.

2. Main Query:

- FROM clause: The delivery_order table is selected as the initial dataset (d alias).

- LEFT JOIN: Performs a left join between the delivery_order table (d alias) and the CTE (high_frequency_cust alias hfc) on customer_id.

- WHERE clause: Applies the order_timestamp filter again to ensure the join is limited to the relevant timeframe.

- SELECT clause:
  
   COUNT(DISTINCT hfc.customer_id): Counts distinct high-frequency customers from the CTE.

   COUNT(DISTINCT d.customer_id): Counts distinct customers who placed any orders.

  ROUND(1.00 * ... / ... * 100, 2): Calculates the percentage of high-frequency customers and rounds it to 2 decimal places.

  AS high_frequency_customer_percentage: Aliases the final calculated result.

# Visual Representation of Execution Order:

1. CTE Execution:

- FROM delivery_order

- WHERE order_timestamp BETWEEN '2023-01-01' AND '2023-01-31' AND order_timestamp IS NOT NULL AND customer_id IS NOT NULL

- GROUP BY customer_id

- HAVING COUNT(*) > 5

2. Main Query:

- FROM delivery_order d

- LEFT JOIN high_frequency_cust hfc ON hfc.customer_id = d.customer_id

- WHERE d.order_timestamp BETWEEN '2023-01-01' AND '2023-01-31'

- SELECT:

  ROUND(1.00 * COUNT(DISTINCT hfc.customer_id) / COUNT(DISTINCT d.customer_id) * 100, 2) AS high_frequency_customer_percentage

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
