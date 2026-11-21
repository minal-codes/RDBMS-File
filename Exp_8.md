| Name | UID | Experiment No. | Experiment Title | SQL Script |
| --- | --- | --- | --- | --- |
| Minal Sharma | `25LBCS3196` | 08 | Transactions, Savepoints and Isolation Levels | [`transactions_isolation.sql`](transactions_isolation.sql) |

## $\color{#0081A7}{\texttt{Aim}}$

Implement banking-style transactions using BEGIN, COMMIT, ROLLBACK, and SAVEPOINT. Simulate partial failures. Demonstrate isolation levels and concurrency anomalies using two sessions.

## $\color{#0081A7}{\texttt{Software Required}}$

- MySQL Server 8.0 or later
- MySQL Workbench or the MySQL command-line client

## $\color{#0081A7}{\texttt{Files}}$

`Exp_8.md` is this lab record; `transactions_isolation.sql` creates the Accounts table and demonstrates transaction control and two-session tests.

How to run:

    mysql -u root -p -t < transactions_isolation.sql

> Run the script on a MySQL 8.0 instance. The SQL file contains the executable statements supplied for this experiment. Capture your own terminal output after running it and add it to the lab record if required.

## $\color{#0081A7}{\texttt{1. Theory}}$

### 1.1 Transaction Control Statements

| Statement | Purpose |
| --- | --- |
| `START TRANSACTION` / `BEGIN` | Starts a transaction. |
| `COMMIT` | Permanently saves the changes made in the transaction. |
| `ROLLBACK` | Cancels uncommitted changes in the current transaction. |
| `SAVEPOINT name` | Creates a point inside a transaction to which execution can roll back. |
| `ROLLBACK TO SAVEPOINT name` | Reverses changes made after the named savepoint without cancelling earlier work. |

### 1.2 ACID Properties

| Property | Meaning |
| --- | --- |
| Atomicity | A transaction succeeds completely or its changes are rolled back. |
| Consistency | A transaction preserves database constraints and valid states. |
| Isolation | Concurrent transactions should not interfere in unintended ways. |
| Durability | Committed changes survive subsequent failures. |

### 1.3 Isolation Levels and Anomalies

| Isolation level | General behaviour |
| --- | --- |
| `READ UNCOMMITTED` | May allow dirty reads. |
| `READ COMMITTED` | Reads only committed data; a repeated read may return a newer committed value. |
| `REPEATABLE READ` | InnoDB's default; ordinary consistent reads use a transaction snapshot. |
| `SERIALIZABLE` | Strongest level; transactions behave as though executed serially, with additional locking. |

- **Dirty read:** a transaction reads another transaction's uncommitted change.
- **Non-repeatable read:** the same row read twice in one transaction returns different committed values.
- **Lost update:** a later write based on stale application data overwrites another transaction's update.

## $\color{#0081A7}{\texttt{2. Implementation}}$

The script creates three bank accounts, performs a committed transfer, demonstrates a full rollback, and uses a savepoint to retain a successful transfer while undoing a later invalid attempt. It then sets each isolation level and provides four two-session schedules: dirty read, non-repeatable read, repeatable-read snapshot behaviour and a stale-data lost-update example.

## $\color{#0081A7}{\texttt{3. Verification and Observations}}$

Run the single-session transaction sections first. For the concurrency demonstrations, open two MySQL sessions and execute the statements in the order marked `SESSION 1` and `SESSION 2`; keep transactions open where the comments instruct. The dirty-read outcome is described as possible and depends on the engine/session behaviour.

## $\color{#0081A7}{\texttt{4. Result}}$

Transaction control statements make banking operations atomic and allow partial rollback through savepoints. The two-session schedules illustrate how isolation levels affect visibility and how stale application data can cause lost updates.
