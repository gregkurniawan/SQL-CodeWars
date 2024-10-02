### Description

Given a demographics table in the following format:

### ** demographics table schema **
- id
- name
- birthday
- race

you need to return the same table where all text fields (name & race) are changed to the bit length of the string.

```sql
SELECT
  id,
  LENGTH(name) * 8 AS name,
  birthday,
  LENGTH(race) * 8 AS race
FROM demographics;
```