# Amazon Quantum Ledger Database - QLDB

- Part of AWS Blockchain part of products
- It as an immutable append-only ledger-only database
- It provides a cryptographically verifiable transaction log
- It is transparent: full history is always accessible
- It is a serverless product, it provides Ledgers and Tables. We have no servers to manage
- It is resilient through 3 AZs, replicates data within each of those AZs
- It can stream data to Amazon Kinesis, it can stream any changes to data into Kinesis in real-time
- It is a document database model, storing JSON documents
- Provides ACID transactions
- Uses cases for QLDB:
    - Anything related to finance: account balances and transactions
    - Medical application: full history of data changes matters
    - Logistics: track movement of objects
    - Legal: track the usage and change of data (custody)
    