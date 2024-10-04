### Description

### Assume you have a PostgreSQL database with a table named "users". The "users" table has the following columns:
- id (integer): a unique identifier for each user.
- name (text): the name of the user.
- birthdate (date): the user's birth date.

The generational categories are generally defined as follows:
- Generation Z: Born from 1997 to 2012 (inclusive)
- Millennials: Born from 1981 to 1996
- Generation X: Born from 1965 to 1980
- Baby Boomers: Born from 1946 to 1964
- Silent Generation: Born from 1928 to 1945
- Greatest Generation: Born in 1927 or earlier

Your SQL query should return all users sorted by id in asc order from the users table with an additional generation field that indicates the generation to which each user belongs based on the above definitions.

Sounds simple? You are not allowed to use the CASE WHEN operator for this task :-)

GLHF!

Desired Output
The desired output should look like this:

id	|       name        | birthdate	 | generation
1	| Mr. Nerissa Cremin| 1994-03-15 | Millennials
2	| Mattie Lind	    | 1961-04-05 | Baby Boomers
3	| Cody Murray	    | 1966-12-05 | Generation X

```sql
WITH generations AS (
    SELECT 
        'Generation Z' AS generation, 
        DATE '1997-01-01' AS start_date, 
        DATE '2012-12-31' AS end_date
    UNION ALL
    SELECT 
        'Millennials', 
        DATE '1981-01-01', 
        DATE '1996-12-31'
    UNION ALL
    SELECT 
        'Generation X', 
        DATE '1965-01-01', 
        DATE '1980-12-31'
    UNION ALL
    SELECT 
        'Baby Boomers', 
        DATE '1946-01-01', 
        DATE '1964-12-31'
    UNION ALL
    SELECT 
        'Silent Generation', 
        DATE '1928-01-01', 
        DATE '1945-12-31'
    UNION ALL
    SELECT 
        'Greatest Generation', 
        DATE '1900-01-01',
        DATE '1927-12-31'
)

SELECT
  u.id,
  u.name,
  u.birthdate,
  g.generation
FROM users u
JOIN generations g ON u.birthdate BETWEEN g.start_date AND g.end_date
ORDER BY u.id;
```