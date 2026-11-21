## Experiments 7–10 (add to README.md)

| Experiment | Topic | Lab record | SQL script |
| --- | --- | --- | --- |
| 7 | Functional dependencies and normalization (UNF to 3NF; BCNF scope noted) | [`Exp_7.md`](Exp_7.md) | [`normalization.sql`](normalization.sql) |
| 8 | Transactions, savepoints and isolation levels | [`Exp_8.md`](Exp_8.md) | [`transactions_isolation.sql`](transactions_isolation.sql) |
| 9 | Single/composite indexing and EXPLAIN benchmarking | [`Exp_9.md`](Exp_9.md) | [`index_benchmark.sql`](index_benchmark.sql) |
| 10 | Query optimization using indexes and query rewriting | [`Exp_10.md`](Exp_10.md) | [`query_optimization.sql`](query_optimization.sql) |

### Run Experiments 7–10

```bash
mysql -u root -p -t < normalization.sql
mysql -u root -p -t < transactions_isolation.sql
mysql -u root -p -t < index_benchmark.sql
mysql -u root -p -t < query_optimization.sql
```

Experiment 8 contains two-session concurrency schedules. Run the statements in the specified order in two separate MySQL sessions; it is not a one-session-only demonstration.
