### Description

Let's say we have a table called sales_data and it contains daily sales data. The table has two columns: day (an integer representing the day) and sales (an integer representing the sales amount for that day).

Sometimes, for whatever reason, sales data for a certain day might be missing (NULL). We want to fill in these missing values with the last recorded sales data.

### Here is a sample sales_data table:
day | sales
----+------
  1 | 100
  2 | 150
  3 | NULL
  4 | NULL
  5 | 200
  6 | 120
  7 | NULL
  8 | NULL
  9 | NULL

Your task is to write a SQL query that returns a result set with the same day values, but where the sales values have been replaced in such a way that each NULL value is replaced by the last non-NULL value (ordered by day). If the first sales values are NULL, they should remain NULL.

### The result set for the sample sales_data table above should look like this:
day | filled_sales
----+-------------
  1 | 100
  2 | 150
  3 | 150
  4 | 150
  5 | 200
  6 | 120
  7 | 120
  8 | 120
  9 | 120

```sql
WITH RECURSIVE FilledSales AS (
    -- Base case: select the first row as is
    SELECT
        day,
        sales,
        sales AS filled_sales
    FROM
        sales_data
    WHERE
        day = 1 -- assuming days start at 1, otherwise modify accordingly
    
    UNION ALL

    -- Recursive case: fill NULL sales values with the last non-NULL value
    SELECT
        sd.day,
        sd.sales,
        COALESCE(sd.sales, fs.filled_sales) AS filled_sales
    FROM
        sales_data sd
    JOIN
        FilledSales fs ON sd.day = fs.day + 1
)
SELECT
    day,
    filled_sales
FROM
    FilledSales
ORDER BY
    day;
```