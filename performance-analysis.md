# Portfolio 2: Query Processing and Optimisation

## 1. Objective

This portfolio investigated SQL query execution and indexing in the Bank/Fintech Platform database using PostgreSQL hosted on Neon.

## 2. Queries and Execution Plans

A customer-and-account query was executed to retrieve customer names, account numbers, account types, and balances. PostgreSQL's `EXPLAIN (ANALYZE, BUFFERS)` command was then used to inspect its execution plan.

A transaction-history query was also examined before and after creating an index on `transactions.account_id`.

## 3. Indexing Experiments

Two indexes were created:

```sql
CREATE INDEX idx_transactions_account_id
ON transactions (account_id);

CREATE INDEX idx_transactions_account_date
ON transactions (account_id, transaction_date DESC);
```

The transaction-history query was tested before and after each index was created. The composite-index experiment showed execution times of 0.94 ms before index creation and 2.9 ms afterwards. The displayed plan continued to use a sequential scan on `transactions` and a separate sort operation.

## 4. Performance Analysis

The experiments demonstrated that creating an index does not guarantee that PostgreSQL will use it. The query planner selects an execution strategy based on estimated costs and the available data.

The database contained very few transaction records, so sequential scans could be more economical than index scans. The observed timing differences are too small to establish a reliable performance improvement or deterioration. Testing with a larger dataset and repeated measurements would provide stronger evidence.

## 5. Evidence

Supporting evidence includes:

- SQL queries and execution-plan screenshots.
- Before-and-after index experiment screenshots.
- The PostgreSQL index inventory.
- The single-column and composite-index analysis documents.

## 6. Conclusion

The exercises provided practical experience in running SQL queries, inspecting execution plans, creating indexes, and evaluating query-planner decisions. The results emphasised the importance of measuring performance rather than assuming that an index will improve every query.
