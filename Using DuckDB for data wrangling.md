---
created: 2025-10-22
---
Good two part series:

https://duckdb.org/2024/08/19/duckdb-tricks-part-1
https://duckdb.org/2024/10/11/duckdb-tricks-part-2

Here are some of  the key items:

|Operation|Snippet|
|---|---|
|[Pretty-printing floats](https://duckdb.org/2024/08/19/duckdb-tricks-part-1#pretty-printing-floating-point-numbers)|`SELECT (10 / 9)::DECIMAL(15, 3)`|
|[Copying the schema](https://duckdb.org/2024/08/19/duckdb-tricks-part-1#copying-the-schema-of-a-table)|`CREATE TABLE tbl AS FROM example LIMIT 0`|
|[Shuffling data](https://duckdb.org/2024/08/19/duckdb-tricks-part-1#shuffling-data)|`FROM example ORDER BY hash(rowid + 42)`|
|[Specifying types when reading CSVs](https://duckdb.org/2024/08/19/duckdb-tricks-part-1#specifying-types-in-the-csv-loader)|`FROM read_csv('example.csv', types = {'x': 'DECIMAL(15, 3)'})`|
|[Updating CSV files in-place](https://duckdb.org/2024/08/19/duckdb-tricks-part-1#updating-csv-files-in-place)|`COPY (SELECT s FROM 'example.csv') TO 'example.csv'`|

| Operation                                                                                                                   | SQL instructions                              |
| --------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| [Fixing timestamps in CSV files](https://duckdb.org/2024/10/11/duckdb-tricks-part-2#fixing-timestamps-in-csv-files)         | `regexp_replace()` and `strptime()`           |
| [Filling in missing values](https://duckdb.org/2024/10/11/duckdb-tricks-part-2#filling-in-missing-values)                   | `CROSS JOIN`, `LEFT JOIN` and `coalesce()`    |
| [Repeated transformation steps](https://duckdb.org/2024/10/11/duckdb-tricks-part-2#repeated-data-transformation-steps)      | `CREATE OR REPLACE TABLE t AS ... FROM t ...` |
| [Computing checksums for columns](https://duckdb.org/2024/10/11/duckdb-tricks-part-2#computing-checksums-for-columns)       | `bit_xor(md5_number(COLUMNS(*)::VARCHAR))`    |
| [Creating a macro for checksum](https://duckdb.org/2024/10/11/duckdb-tricks-part-2#creating-a-macro-for-the-checksum-query) | `CREATE MACRO checksum(tbl) AS TABLE ...`     |

```sql
CREATE TABLE schedule_cleaned AS SELECT timeslot .regexp_replace(' (\d+)(am|pm)$', ' \1.00\2') .strptime('%Y-%m-%d %H.%M%p') AS timeslot, location, event FROM schedule_raw;
```

###  Repeated Data Transformation Steps

Nice tip on how to run processes.

> Data cleaning and transformation usually happens as a sequence of transformations that shape the data into a form that’s best fitted to later analysis. These transformations are often done by defining newer and newer tables using [`CREATE TABLE ... AS SELECT` statements](https://duckdb.org/docs/stable/sql/statements/create_table.html#create-table--as-select-ctas).
> 
> For example, in the sections above, we created `schedule_raw`, `schedule_cleaned`, and `schedule_filled`. If, for some reason, we want to skip the cleaning steps for the timestamps, we have to reformulate the query computing `schedule_filled` to use `schedule_raw` instead of `schedule_cleaned`. This can be tedious and error-prone, and it results in a lot of unused temporary data – data that may accidentally get picked up by queries that we forgot to update!
> 
> In interactive analysis, it’s often better to use the same table name by running [`CREATE OR REPLACE` statements](https://duckdb.org/docs/stable/sql/statements/create_table.html#create-or-replace):
> 
> ```sql
> CREATE OR REPLACE TABLE table_name AS
>     ...
>     FROM table_name
>     ...;
> ```

## Questions

One question I still have is:

How I would run this in e.g. GitHub Actions? How would i store the code, chain steps together. Do I use something like dbt or can I just use a simpler way?

## Ok, let's try it out ...

Have grants table here ...

```sql
create table grants2 as select Grant, Amount.replace('$', '').replace(',', '') as Amount from grants;

alter table grants2 alter Amount type int;

select sum(Amount) from grants2;
```

total is: 3,703,705,877 ($3.7bn)