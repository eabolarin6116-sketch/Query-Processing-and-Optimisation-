# PostgreSQL Index Inventory

## Objective

The purpose of this exercise was to inspect the indexes defined in the Bank/Fintech Platform database and identify the indexes available to PostgreSQL when executing queries.

## Method

The PostgreSQL system view `pg_indexes` was queried to display the schema, table name, index name, and SQL definition of indexes associated with the database's main tables.

## Findings

The index inventory was displayed successfully. The inventory should be reviewed to identify:

- Primary-key indexes.
- Unique-constraint indexes, including the account-number index.
- The manually created `idx_transactions_account_id` index.
- The manually created `idx_transactions_account_date` composite index.

## Relevance to Query Optimisation

Indexes can help PostgreSQL locate records efficiently, but the query planner chooses whether to use them based on estimated costs and the query's requirements. In the previous experiments, the transaction-history queries continued to use sequential scans on the small `transactions` table despite the additional indexes.

## Evidence

A screenshot of the SQL query and its results is retained as supporting evidence.
