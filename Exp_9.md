| Name | UID | Experiment No. | Experiment Title | SQL Script |
| --- | --- | --- | --- | --- |
| Minal Sharma | `25LBCS3196` | 09 | Indexing and Query Performance Benchmarking | [`index_benchmark.sql`](index_benchmark.sql) |

## $\color{#0081A7}{\texttt{Aim}}$

Create a Railway Booking table with at least 5,000 records. Benchmark query performance before and after creating single and composite indexes. Use EXPLAIN to compare execution plans.

## $\color{#0081A7}{\texttt{Software Required}}$

- MySQL Server 8.0 or later
- MySQL Workbench or the MySQL command-line client

## $\color{#0081A7}{\texttt{Files}}$

`Exp_9.md` is this lab record; `index_benchmark.sql` creates 5,000 railway bookings, runs baseline queries, adds indexes and repeats the benchmarks.

How to run:

    mysql -u root -p -t < index_benchmark.sql

> Run the script on a MySQL 8.0 instance. The SQL file contains the executable statements supplied for this experiment. Capture your own terminal output after running it and add it to the lab record if required.

## $\color{#0081A7}{\texttt{1. Theory}}$

### 1.1 Database Index

An index is an auxiliary data structure that helps MySQL locate rows without scanning the entire table. It can reduce read time, but it consumes storage and adds work to `INSERT`, `UPDATE` and `DELETE` operations.

### 1.2 Single-Column and Composite Indexes

| Index type | Description | Example in this experiment |
| --- | --- | --- |
| Single-column | Built on one column. | `train_no`, `source_station`, `journey_date` |
| Composite | Built on an ordered combination of columns. | `(source_station, journey_date)` and `(train_no, journey_date, booking_status)` |

The leftmost-prefix rule matters: a composite index is most useful for predicates that begin with its leading column(s).

### 1.3 EXPLAIN and ANALYZE TABLE

`EXPLAIN` displays the optimizer's planned access method, possible indexes, chosen key and estimated rows. `ANALYZE TABLE` refreshes table statistics used by the optimizer. `EXPLAIN ANALYZE` executes the query and reports actual iterator timing and row information on supported MySQL versions.

### 1.4 Measuring Execution Time

The script records a timestamp with `NOW(6)`, runs a `SQL_NO_CACHE` count query, and calculates elapsed microseconds with `TIMESTAMPDIFF`. Timings are environment-dependent and should be treated as observations, not guaranteed performance values.

## $\color{#0081A7}{\texttt{2. Implementation}}$

A stored procedure generates 5,000 deterministic booking rows. Three representative queries are examined: search by train number; search by source station and journey date; and search by train number, date and booking status. The script measures the baseline, creates single-column indexes, measures again, then adds composite indexes and performs a final comparison.

## $\color{#0081A7}{\texttt{3. Verification and Observations}}$

- `total_records` should be 5000.
- Review `EXPLAIN` output before and after indexing, especially `type`, `possible_keys`, `key` and `rows`.
- `SHOW INDEX FROM Railway_Booking` lists the created indexes.
- Compare measured microseconds on the same machine and database state; small timing differences may be noise.

## $\color{#0081A7}{\texttt{4. Result}}$

The experiment demonstrates how single-column and composite indexes can change optimizer access plans and reduce the number of rows examined for selective predicates. Actual timing improvements depend on data distribution, server load, cache state and optimizer decisions.
