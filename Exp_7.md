| Name | UID | Experiment No. | Experiment Title | SQL Script |
| --- | --- | --- | --- | --- |
| Minal Sharma | `25LBCS3196` | 07 | Functional Dependencies and Database Normalization | [`normalization.sql`](normalization.sql) |

## $\color{#0081A7}{\texttt{Aim}}$

Given a denormalized Sales table, identify functional dependencies and normalize step-by-step from UNF to 3NF and BCNF. Verify lossless join and dependency preservation using SQL queries.

## $\color{#0081A7}{\texttt{Software Required}}$

- MySQL Server 8.0 or later
- MySQL Workbench or the MySQL command-line client

## $\color{#0081A7}{\texttt{Files}}$

`Exp_7.md` is this lab record; `normalization.sql` creates the UNF, 1NF, 2NF and 3NF structures and runs reconstruction and key checks.

How to run:

    mysql -u root -p -t < normalization.sql

> Run the script on a MySQL 8.0 instance. The SQL file contains the executable statements supplied for this experiment. Capture your own terminal output after running it and add it to the lab record if required.

## $\color{#0081A7}{\texttt{1. Theory}}$

### 1.1 Functional Dependencies

A functional dependency (FD) `X → Y` means that a value of attribute set `X` uniquely determines a value of attribute set `Y`. The left side is the determinant.

| Functional dependency | Meaning in the Sales example |
| --- | --- |
| `Order_ID → Order_Date, Customer_ID` | An order identifies its date and customer. |
| `Customer_ID → Customer_Name, Customer_City` | A customer ID identifies customer details. |
| `Product_Name → Unit_Price` | A product identifies its unit price. |
| `(Order_ID, Product_Name) → Quantity` | An order-product pair identifies the ordered quantity. |

### 1.2 Normal Forms

| Form | Requirement | Application here |
| --- | --- | --- |
| UNF | Repeating groups or non-atomic values may exist. | `Products` stores multiple product entries in one field. |
| 1NF | Every attribute contains an atomic value; no repeating groups. | One row is stored for each order-product combination. |
| 2NF | 1NF and every non-prime attribute depends on the whole candidate key, not part of a composite key. | Order, product and order-detail data are separated. |
| 3NF | 2NF and no transitive dependency of non-prime attributes on a candidate key. | Customer details are moved to `Customers_3NF`. |
| BCNF | For every non-trivial FD `X → Y`, `X` must be a superkey. | The supplied SQL builds through 3NF; it does not show a separate BCNF decomposition. |

### 1.3 Lossless Join and Dependency Preservation

A decomposition is lossless when joining the decomposed relations reconstructs exactly the original relation without spurious rows. Dependency preservation means the original functional dependencies can be checked/enforced on the decomposed relations without joining them. The script compares the reconstructed 3NF row count with the original 1NF row count and checks duplicate determinants using `GROUP BY` / `HAVING`.

**Scope note:** The supplied experiment states BCNF in its aim, but its implementation creates and verifies 1NF, 2NF and 3NF tables only. It does not provide an explicit BCNF decomposition or a formal dependency-preservation proof. The record preserves that distinction rather than claiming an unperformed BCNF step.

## $\color{#0081A7}{\texttt{2. Implementation}}$

The SQL script creates the denormalized `Sales_UNF` table, converts repeating product entries into atomic rows in `Sales_1NF`, separates order/product/order-detail relations for 2NF, and further separates customer attributes for 3NF. It then reconstructs the sales rows using joins, compares row counts, checks the stated functional dependencies, inspects table definitions and counts the resulting records.

## $\color{#0081A7}{\texttt{3. Verification and Observations}}$

- `Reconstructed_Rows` should equal `Original_Rows` (6 rows in the supplied sample).
- Duplicate-determinant queries should return no rows because each determinant is a primary key in its relation.
- Final table counts: 2 customers, 3 orders, 4 products and 6 order details.

## $\color{#0081A7}{\texttt{4. Result}}$

The Sales data is transformed from a denormalized structure through 1NF, 2NF and 3NF. The join reconstructs the six original order-product rows, and primary-key checks support the listed functional dependencies. A separate BCNF decomposition would be required to fully demonstrate the BCNF part of the stated aim.
