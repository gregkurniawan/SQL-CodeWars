### Description:
Imagine you are working with a sports analytics company that tracks the performance of runners in various races. The company has a PostgreSQL database with three tables: runners, runs, and races. Your task is to create a report that lists all the races a specific runner (runner_id  = 1) participated in and includes additional insights about their competitors.

### runners:
- runner_id (int): primary key
- name (varchar): The name of the runner

### runs:
- run_id (int): primary key
- runner_id (int, foreign key to runners): ID of the runner who participated in the run
- race_id: (int, foreign key to races): ID of the race in which the run took place
- position (int): The finishing position of the runner in the race

### races:
- race_id (int): primary key
- date (date): The date when the race took place

### Create a query that lists all the races a given runner (runner_id  = 1) participated in, including the following columns:
- runner_name: The name of the runner
- run_id: The unique identifier for the run
- position: The finishing position of the runner in the race
- race_id: The unique identifier for the race
- win/second: A new column that:
    - Displays the name and ID (in the format of name (id) of the runner who finished second if the current runner's position is 1
    - Displays the name and ID (in the format of name (id) of the winning runner in that race if the current runner's position is not 1

The results should be ordered by the race date in ascending order. It is not possible to have multiple races on the same date so thus second ordering criterion is not needed.

### For this sample data:

### runners
| runner_id | name          |
+-----------+---------------+
|         1 | John Runner   |
|         2 | Second Runner |
|         3 | Winning Runner|

### races
| race_id | date                |
+---------+---------------------+
|       1 | 2024-07-01          |
|       2 | 2024-07-02          |
|       3 | 2024-07-03          |

### runs
| run_id | runner_id | race_id | position |
+--------+-----------+---------+----------+
|      1 |         1 |       1 |        1 |
|      2 |         2 |       1 |        2 |
|      3 |         3 |       1 |        3 |
|      4 |         1 |       2 |        3 |
|      5 |         2 |       2 |        1 |
|      6 |         3 |       2 |        2 |
|      7 |         1 |       3 |        2 |
|      8 |         2 |       3 |        3 |
|      9 |         3 |       3 |        1 |

### the desired output is the following:
| runner_name  | run_id | position | race_id | win/second        |
+--------------+--------+----------+---------+-------------------+
| John Runner  |      1 |        1 |       1 | Second Runner (2) |
| John Runner  |      4 |        3 |       2 | Second Runner (2) |
| John Runner  |      7 |        2 |       3 | Winning Runner (3)|

```sql
SELECT
  r.name runner_name,
  rn.run_id,
  rn.position,
  rc.race_id,
  CASE
    WHEN rn.position= 1 THEN (
      SELECT CONCAT(r3.name,' (',rn3.runner_id,')')
      FROM runs rn3
      JOIN runners r3 ON rn3.runner_id=r3.runner_id
      JOIN races rc3 ON rc3.race_id=rn3.race_id
      WHERE rn3.position=2 AND rc3.race_id=rc.race_id  
    )
    ELSE (
      SELECT CONCAT(r2.name,' (',rn2.runner_id,')')
      FROM runs rn2
      JOIN runners r2 ON rn2.runner_id=r2.runner_id
      JOIN races rc2 ON rc2.race_id=rn2.race_id
      WHERE rn2.position=1 AND rc2.race_id=rc.race_id
    )
  END AS "win/second"
FROM runners r
JOIN runs rn ON r.runner_id=rn.runner_id
JOIN races rc ON rc.race_id=rn.race_id
WHERE rn.runner_id = 1
```