| Name | UID | Experiment No. | Experiment Title | SQL Script |
| --- | --- | --- | --- | --- |
| Minal Sharma | `25LBCS3196` | 10 | Query Optimization Using Indexing and Query Rewriting | [`query_optimization.sql`](query_optimization.sql) |

## $\color{#0081A7}{\texttt{Aim}}$

Generate a dataset with at least 10,000 rows. Write five slow queries, analyze using EXPLAIN, optimize using indexing and query rewriting, and compare execution time before and after optimization.

## $\color{#0081A7}{\texttt{Software Required}}$

- MySQL Server 8.0 or later
- MySQL Workbench or the MySQL command-line client

## $\color{#0081A7}{\texttt{Files}}$

`Exp_10.md` is this lab record; `query_optimization.sql` creates 10,000 sales rows and compares five queries before and after indexing and rewriting.

How to run:

    mysql -u root -p -t < query_optimization.sql

> Run the script on a MySQL 8.0 instance. The SQL file contains the executable statements supplied for this experiment. Capture your own terminal output after running it and add it to the lab record if required.

## $\color{#0081A7}{\texttt{1. Theory}}$

### 1.1 Query Optimization

Query optimization is the process of selecting an efficient execution strategy for an SQL statement. The MySQL optimizer considers available indexes, predicates, table statistics, join order and estimated cost.

### 1.2 Indexing Strategies

| Index | Query supported |
| --- | --- |
| `(region, status)` | Filtering by region and status. |
| `sale_date` | Date-range filtering. |
| `(category, region)` | Filtering by category and region. |
| `region` | Grouping/reading by region where the optimizer can benefit. |
| `customer_id` | Grouping sales by customer. |

### 1.3 SARGable Predicates and Query Rewriting

A SARGable predicate allows an index to be used efficiently for searching. Applying a function to an indexed column, such as `YEAR(sale_date)`, can prevent a normal range lookup. Rewriting it as a half-open date range (`sale_date >= ... AND sale_date < ...`) makes the condition more index-friendly and avoids end-of-period boundary problems.

### 1.4 EXPLAIN and Timing

`EXPLAIN` shows the optimizer's chosen access path and estimated row count. `EXPLAIN ANALYZE` provides execution details on supported MySQL versions. The script measures each query with microsecond timestamps before and after optimization; results vary by environment.

## $\color{#0081A7}{\texttt{2. Implementation}}$

The script generates 10,000 sales records. It benchmarks five queries: region/status filtering, date-range filtering, category/region filtering, revenue aggregation by region and customer spending aggregation with `HAVING`. It then creates single and composite indexes, refreshes statistics, repeats EXPLAIN and timing, and compares a non-SARGable `YEAR(sale_date)` condition with an equivalent date range.

## $\color{#0081A7}{\texttt{3. Verification and Observations}}$

- `total_records` should be 10000.
- Inspect the selected `key`, access `type` and estimated `rows` in each `EXPLAIN` result.
- Compare the `q*_before_microseconds` and `q*_after_microseconds` values.
- The date-range rewrite should be checked against the `YEAR(sale_date)` version for equivalent date coverage.

## $\color{#0081A7}{\texttt{4. Result}}$

The experiment shows how composite and single-column indexes, refreshed optimizer statistics and SARGable date predicates can improve query execution plans. Aggregation queries may still require substantial work because indexes do not eliminate the cost of calculating grouped sums.
