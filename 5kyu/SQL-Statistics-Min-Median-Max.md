### Description

For this challenge you need to create a simple SELECT statement. Your task is to calculate the MIN, MEDIAN and MAX scores of the students from the results table.

### Resultant table:
- min
- median
- max

```sql
SELECT
  MIN(score) min,
  PERCENTILE_CONT(0.5) WITHIN GROUP(ORDER BY score DESC) median,
  MAX(score) max
FROM result;
```