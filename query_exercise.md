# Query Exercise
The following self-contained query is designed to run on Postgres, but can be easily refactored to run on any database engine that supports CTEs.

The `person` CTE defines people with a unique `id`, their `age`, and a `group_id` value that corresponds to some unknown external group that's just used for grouping purposes:

```sql
with
  person as (
    select 1 as id, 60 as age,  2 as group_id union all
    select 2,       67,         3             union all
    select 3,       50,         3             union all
    select 4,       57,         2             union all
    select 5,       36,         2             union all
    select 6,       61,         4             union all
    select 7,       50,         2             union all
    select 8,       27,         4             union all
    select 9,       21,         1             union all
    select 10,      46,         2             union all
    select 11,      55,         4             union all
    select 12,      21,         4             union all
    select 13,      40,         2             union all
    select 14,      59,         1             union all
    select 15,      61,         2             union all
    select 16,      55,         2             union all
    select 17,      19,         3             union all
    select 18,      31,         3             union all
    select 19,      24,         1             union all
    select 20,      71,         4
  )
select
  group_id,
  count(*) as person_count,
  avg(age) as average_age
from
  person
group by
  group_id
order by
  group_id
```

In its current form, the query output is:

|group_id|person_count|average_age|
|-------:|-----------:|----------:|
|1|3|34.6666666666666667|
|2|8|50.625|
|3|4|41.75|
|4|5|47|


## **Objective**
Rewrite the main `select` portion of the query to instead select the second oldest person in each group and display the age and ID of that person. In other words, the output should be:

|group_id|age|person_id|
|-------:|--:|--------:|
|1|24|19|
|2|60|1|
|3|50|3|
|4|61|6|

If you want to support edge cases (only 1 person in a group, for example), feel free, but that should not be considered a primary objective.

If you don't have convenient access to a database engine, [https://rextester.com/l/postgresql_online_compiler](https://rextester.com/l/postgresql_online_compiler) is a good paste-and-execute SQL evaluator that has been verified to execute the above query. [http://sqlfiddle.com](http://sqlfiddle.com) is also a good option, but requires building up the schema to query against separately.
