### Description

You are given a PostgreSQL table named customers, which keeps track of customer visits. 

### The schema for the customers table is as follows:
- date (date): The date when the customer visited.
- customer_id (integer): The unique ID for each customer.
- name (text): The name of the customer.

### Here is some sample data:
date      |customer_id| name   
----------+-----------+-------
2023-08-01| 1         | Andrew 
2023-08-02| 2         | Pete   
2023-08-03| 2         | Andrew 
2023-08-04| 2         | Steve  
2023-08-05| 2         | Stef   
2023-08-06| 3         | Stef  
2023-08-07| 1         | Jason 
2023-08-08| 1         | Jason 

Your task is to write an SQL query that, for each row in the table, calculates the count of distinct names within the past two days (including the current day) for the same customer_id. The result should be ordered by the date in ascending order, then by customer_id in descending order, and finally by name in ascending order.

The output of your query should be all columns from customers table and count of integer type where the count of distinct names is shown that share the same customer_id within the past two days (including the current day).

### So thus the result of your query from the sample data abobe should look something like this:
date      |customer_id| name  |count 
----------+-----------+-------+------
2023-08-01| 1         |Andrew | 1 
2023-08-02| 2         |Pete   | 1 
2023-08-03| 2         |Andrew | 2 
2023-08-04| 2         |Steve  | 3 
2023-08-05| 2         |Stef   | 3 
2023-08-06| 3         |Stef   | 1 
2023-08-07| 1         |Jason  | 1 
2023-08-08| 1         |Jason  | 1 

```sql
SELECT 
    c1.date,
    c1.customer_id,
    c1.name,
    (SELECT COUNT(DISTINCT c2.name)
        FROM customers c2
        WHERE c2.customer_id = c1.customer_id
        AND c2.date BETWEEN c1.date - INTERVAL '2 days' AND c1.date
    ) AS count
FROM 
    customers c1
ORDER BY 
    c1.date ASC,
    c1.customer_id DESC,
    c1.name ASC;
```