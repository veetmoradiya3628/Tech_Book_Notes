
ACID internals
- WAL - Write Ahead Log
	- Every change appends to a redo record to a WAL and calls fsync before returning COMMIT OK
	- 16MB by default
- MVCC - multi-version concurrency control
	- Postgres stores old row versions in-place in the heap. Each tuple carries xmin (creating transaction ID) and xmax (deleting / updating transaction ID) 
	- A visibility check per query decides which versions this snapshot can see
	- Dead tuple accumulate until VACCUM reclaims them

Isolation levels

| Level                                  | Anomalies prevented              | Mechanism                                           |
| -------------------------------------- | -------------------------------- | --------------------------------------------------- |
| Read Committed (default)               | Dirty reads                      | Fresh snapshot per statement                        |
| Repeatable Read (= Snapshot Isolation) | + non-repeatable reads, phantoms | Single snapshot for entire transaction              |
| Serializable (= SSI)                   | + write skew                     | Runtime cycle detection, aborts with SQLSTATE 40001 |
Indexes
- An index is an auxiliary data structure that converts O(n) scans into O(log n) lookups.
- B-tree
- Hash
- GIN
- GiST
- SP-GiST
- BRIN
- Bloom (extension)
