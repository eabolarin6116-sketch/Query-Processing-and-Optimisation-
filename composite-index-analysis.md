Composite Index Analysis

1. Objective

This experiment investigated whether a composite index on account_id and transaction_date could improve a query that retrieves an account's transactions in newest-first order.

2. Index Created

CREATE INDEX idx_transactions_account_date
ON transactions (account_id, transaction_date DESC);

3. Query Tested

SELECT
    transaction_id,
    transaction_type,
    amount,
    status,
    transaction_date
FROM transactions
WHERE account_id = (
    SELECT account_id
    FROM accounts
    WHERE account_number = 'FIN100001'
)
ORDER BY transaction_date DESC;## 4. Results

| Measurement | Before index | After index |
|---|---|---|
| Execution time | 0.94 ms | 2.9 ms |
| Planning time | 16 ms | 21.9 ms |
| Transactions access method | Sequential Scan | Sequential Scan |
| Accounts access method | Index Scan | Index Scan |
| Separate Sort operation | Yes | Yes |
| Composite index used | No  | No |

## 5. Discussion

Before the composite index was created, PostgreSQL used a sequential scan on the `transactions` table and an index scan on `accounts` to identify the account. A separate sort operation arranged transactions by transaction date in descending order.

After the composite index was created, PostgreSQL still used a sequential scan on `transactions` and retained the separate sort operation. The new composite index was not visibly used by the displayed execution plan.

Execution time increased from 0.94 ms to 2.9 ms, while planning time increased from 16 ms to 21.9 ms. Since the dataset contains very few transactions, these measurements do not establish that the index worsened performance. The results demonstrate that the query planner may choose not to use an available index when scanning a small table is estimated to be cheaper.

## 6. Conclusion

The experiment demonstrated how to create a composite index and inspect its effect on a PostgreSQL execution plan. In this test, the index did not change the visible access method or eliminate the sort operation. Further testing with a larger dataset would be needed to evaluate its potential performance benefits.
