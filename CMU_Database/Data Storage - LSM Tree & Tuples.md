
- Record IDs
	- The DBMS assigns each logical tuple a unique record identifier that represents its physical location in the database.
	- File Id, page Id, slot \#
	- Most DBMSs do not store ids in tuple
	- SQLite uses RowId as a true primary key and stores them as a hidden attributes.
	- PostgreSQL to see ctid (record Id) we can get by r.ctid column.
- Tuple oriented storage
	- Insert a new tuple
	- Update an existing tuple using its record Id
	- Problems
		1. Fragmentation
		2. Useless Disk I/O
		3. Random Disk I/O
- What if the DBMS cannot overwrite data in pages and could only create new pages ?
	- Examples: Some Object stores (s3), HDFS, Google colossus
- Log-structured storage
	- Instead of storing tuple in pages and updating the in-place, the DBMS maintains a log that records changes to tuples.
		- Each log entry represents a tuple PUT/DELETE operation.
	- The DBMS applied changes to an in-memory data structure (MemTable) and then writes out the changes sequentially to disk (SSTable).
	- Summary Table - has information on SSTable Min/Max key & Key filter per level
	- Widely used for Key Value storage
	- LS compaction - using sort-merge algorithm.
	- Cons
		- Write-Amplification
		- Compaction is expensive
- Index-organized storage
	- DBMS stores a table's tuples as the value of an index data structure.
-  Tuple Storage
	- A tuple is essentially a sequence of bytes prefixed with a header that contains meta-data about it.
	- It it the job of the DBMS to interpret those bytes into attribute types and values.
	- The DBMS's catalogs contain the schema information about tables that the system uses to figure out the tuple's layout.
- 