Indexing and Query Performance Analysis

 1. Objective

The objective of this experiment was to investigate how an index on `transactions.account_id` affects the execution plan of a transaction-history query in the Bank/Fintech Platform database.

 2. Index Created

```sql
CREATE INDEX idx_transactions_account_id
ON transactions (account_id);
```

The index was created successfully in PostgreSQL.

3. Experimental Method

The same transaction-history query was executed before and after creating the index. PostgreSQL's `EXPLAIN (ANALYZE, BUFFERS)` command was used to inspect the execution plan and measure execution time.

 4. Results

| Measurement | Before index | After index |
|---|---|---|
| Execution time | 0.101 ms | 2.38 ms |
| Planning time | 0.112 ms | 21.6 ms |
| Transactions access method | Sequential Scan | Sequential Scan |
| Accounts access method | Index Scan | Index Scan |
| Join method | Hash Join | Nested Loop |
| Index `idx_transactions_account_id` used | No | No |

5. Discussion

The before-index execution plan used a sequential scan on the `transactions` table, an index scan on `accounts` using `accounts_account_number_key`, and a hash join. The after-index plan used a sequential scan on `transactions`, an index scan on `accounts`, and a nested-loop join.

The newly created index on `transactions.account_id` was not used in either displayed plan. The measured execution time increased from 0.101 ms to 2.38 ms, while planning time increased from 0.112 ms to 21.6 ms. Since the dataset is very small, these measurements are insufficient to conclude that the index reduced or worsened overall performance. A larger dataset and repeated measurements would provide a more meaningful comparison.
