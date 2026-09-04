
- Redis is a memory-first, key-value data store.
	- In memory storage provides unparalleled data access speed
	- data stays as long as you need it
	- standard data structures like strings, hashes, lists, JSON, Vectors
- Use cases
	- Enterprise caching
	- Session management
	- Real-time Leaderboards
	- Vector search
- Redis products
	- Redis open source
	- Redis cloud
	- Redis software
- Redis cloud database setup & connect setup

- Keys, values and strings
	- Value - A piece of data you want to store
	- Key - Name or label for your data
	- SET K V
	- GET K
	- UNLINK K
	- Naming keys
		- use a consistent naming convention to avoid confusion and collisions
	- Key spaces
		- Group related keys together using a hierarchical set of prefixes
		- Ex
			```
			SET btc:config:payment:hostname abc.com
			SET btc:config:payment:port 8080
			```
	- Strings: more than plain text
		- store, increment, decrement numbers
		- get substrings within a string
		- conduct bitwise operations
	- Strings are binary safe sequence of bytes
- Lists
	- An ordered group of elements
	- LPUSH
	- RPUSH
	- LPOP
	- RPOP
	- LRANGE
		- supports negative indexes
	- LINDEX
	- List additionally supports
		- moving elements between lists
		- removing ranges of elements
		- finding matching elements
	- head & tail concept
	```
	LPUSH products:recent:alice BOWTIE12
	
	LPUSH products:recent:alice BOWTIE134
	
	LPUSH products:recent:alice BOWTIE134
	```
	- Use cases
		- When order matters
			- tracking a user's previously viewed products
		- As a message queue
			- background task processing
		- As a stack
			- breadcrumb trail on a website
- Sets
	- Unordered collection of unique elements
	- SADD
	- SREM 
	- SMEMBERS \<key>
	- SCARD \<key>
	- Set operations
		- it also supports mathematical set operations like union, intersection and difference
```
SADD product:views:bowtie42 alice - return 1
SADD product:views:bowtie42 bob chuck dave - return 3
SADD product:views:bowtie42 alice - return 0
```
- Use cases
	- store information when uniqueness matters
	- move members between set
	- get random members of a set
	- store unions, intersections and differences

- Hashes
	- A collection of key-value pairs
	- Redis has keys and hashes have fields
	- HSET
	```
	HSET <key> <fiedl1-k> <field1-v> <fiedl2-k> <field2-v>
	```
	- Expire fields
	- store and increment numbers
	- return random fields
	- remove fields
- Use cases
	- Session data
	- records like user profiles
	- cache the records
- index, search use cases

- Sorted sets
	- A set where each member is associated with a score
		- ZADD \<key> score1 item1
		- ZADD \<key> score1 item1 score2 item2 score3 item 3
		- ZSCORE \<key> item
		- ZRANK \<key> item
		- ZRANGE - to find items between score range l to r
	- Use cases
		- Leaderboards
		- Recommendation engines

- JSON
	- JSON serialized string vs. JSON document
	- It supports query and manipulate parts of the document using JSONPath
	- JSON.SET $
	- JSON.GET
	- JSON.GET $.\<field>  or $.*
	- Merge JSON documents
	- Work with multiple JSON documents
	- Manipulate in many ways
	- Use cases
		- To store records
		- To store sessions data
	- Index, search

- Probabilistic data structure
	- A data structure that sacrifices accuracy to gain improvements in speed and storage
	- Hyper log log
		- A data structure that counts a practically unlimited number of unique items
	- Bloom filter
		- A fast and space-efficient data structure that checks a set for membership
- Streams
	- An ordered data structure recording a series of chronological events and their associated data
- Geospatial indexes
	- A searchable collection of named locations storing longitude and latitude
- Storing vectors in Redis for semantic search, semantic caching, RAG
- Bitmaps, Bitfields, TimeSeries, Probabilistic

- Key Expiration
	- Two types of keys
		- persistent
		- volatile
	- TTL
	- EXPIRE to set TTL for key
	- EXPIREAT to set unix timestamp for key expiry
	- stale data expiry is main use case
	- if key is expire get will give -2

- Additional use cases
	- Caching
	- Search and Query
	- Session management
	- Vector Search

- Redis data structures
	- Strings - text
	- Bitmaps - Bitmap encoding
	- Bit field - Efficient integers
	- Hashes - Sessions / profiles
	- Lists - Queues
	- Sets - Recommendations
	- Sorted sets - Leaderboards
	- Geospatial indexes - Location services
	- Hyper-log-log - statistical estimations
	- Streams - Event stream processing

- HyperLogLog - HLL
	- Utilized to estimate the cardinality - the number of unique elements - of a massive, rapidly growing dataset.
	- It operates with 0.81% of error rate which is really minimal for huge dataset
	- The 0.81% margin of error makes this structure ideal for large-scale analytics where absolute exactness is not a rigid business requirement.

- Geo Hash
	- Internally Sorted set data structure is used
	- Provides geo search capability etc
	- Useful for geo operation
	- Converts latitude, longitude into geohash

- Pipelining
- Transactions
- Server side scription with Lua Scripting
	- Cryptographic script caching mechanism
	- ARGV & KEYS
	- SHA
	- EVAL & other methods

- Cache Eviction and Memory management
	- maxmemory directive in redis.conf file for maximum memory redis can utilize
	- there are different policies for eviction
	- read operations will continue but when reached maximum memory limit write operations will fail with OOM error 
- Allkeys vs. Volatile
	- Allkeys scope
		- The policy evaluates every single key in the database for potential eviction, regardless of whether it was intended to be a permanent record or a temporary cache entry.
	- Volatile scope
		- The policy restricts eviction solely to keys that have an explicit Time-To-Live (TTL) expiration set. Keys without a TTL are considered permanent data and are completely protected from eviction algorithms.
- Eviction algorithms
	- Redis employs three primary mathematical algorithms to determine which specific key to delete within the chosen scope
		- Least Recently Used (LRU)
		- Least Frequently Used (LFU)
		- Time-to-live (TTL)

### Redis Eviction Policies

| Eviction Policy  | Target Scope  | Eviction Algorithm Logic                                            | Optimal Use Case                                                                                                |
| :--------------- | :------------ | :------------------------------------------------------------------ | :-------------------------------------------------------------------------------------------------------------- |
| `allkeys-lru`    | Every key     | Evicts the key that has not been accessed for the longest time.     | General-purpose caching following the Pareto principle (recently accessed data is likely to be accessed again). |
| `volatile-lru`   | TTL keys only | Evicts the oldest untouched key among those set to expire.          | Shared instances acting as both a permanent datastore and a temporary cache.                                    |
| `allkeys-lfu`    | Every key     | Evicts the key with the lowest historical frequency of access.      | Application caches where specific data remains globally popular despite brief lulls in access.                  |
| `volatile-lfu`   | TTL keys only | Evicts the least frequently accessed key among those set to expire. | Caches prioritizing popularity over recency.                                                                    |
| `volatile-ttl`   | TTL keys only | Evicts the key with the shortest remaining lifespan.                | Workloads where developers provide accurate hints via TTL regarding a key's long-term utility.                  |
| `allkeys-random` | Every key     | Evicts a key entirely at random.                                    | Cyclic workloads where access patterns are completely uniform.                                                  |
| `noeviction`     | None          | Returns an error on write attempts.                                 | Strict databases where data loss is unacceptable.                                                               |

- Clustering
	- Redis relies on the concept called HashSlots for distributed sharing
	- The global space is divided into 16384 distinct slots
	- CRC16 hash with % 16384 decides keys goes to which slot and that slot belongs to which redis shard (physical instance) will be decided by cluster routing logic
	- Multi key restriction on transaction, lua scripting, if all keys not belong to same physical host slots operation will be rejected by error CROSSLOT 
	- To avoid this error we can employ HashTags mechanism

- Debugging tool in production
	- MONITOR: Streams back every command processed by the Redis server in real-time. While invaluable for finding rogue application commands or connection leaks, it is highly CPU-intensive and reduces server throughput by over 50%, meaning it must be used with extreme caution in production environments.
	- INFO: Returns comprehensive server statistics, including memory consumption, cache hit/miss ratios, connected client counts, and replication synchronization status.
	- OBJECT: Used to inspect the internal, low-level encoding of a specific key (e.g., determining whether a Hash is stored as a memory-efficient ziplist or a standard hashtable) and its idle time for LRU eviction analysis.

- Probabilistic DS
1. HyperLogLog (HLL)
	- Estimates the unique cardinality (number of distinct elements) of a dataset
	- 12KB per key fixed memory
	- 0.81% Error rate
	- Sparse vs. Dense
2. Bloom Filter
	- Answers the question "Is this element definitively NOT in the set, or PROBABLY in the set?"
	- No false negative, if it says an item doesn't exist, it is 100% accurate
3. Cuckoo Filter
	- Similar to a Bloom Filter (membership testing) but supports deletion and is more memory-efficient at extremely low false-positive rates.
4. Count-Min Sketch
	- Estimates the frequency (count) of events in a data stream. Excellent for finding "heavy hitters" or tracking views/clicks.
5. Top-K
	- Identifies the $K$ most frequent items in a stream in real-time, in constant memory.
6. t-digest
	- Accurately estimates quantiles/percentiles (e.g., p95, p99) of continuous numeric data streams.

| Data Structure    | Answers the Question             | Module      | Supports Deletion? | Primary Exam Metric                 |
| :---------------- | :------------------------------- | :---------- | :----------------- | :---------------------------------- |
| **HyperLogLog**   | "How many distinct items?"       | Core Redis  | No                 | Fixed 12KB max size                 |
| **Bloom Filter**  | "Have I seen this item?"         | Redis Stack | No                 | No False Negatives                  |
| **Cuckoo Filter** | "Have I seen this item?"         | Redis Stack | **Yes**            | Handles deletion, high fill-rate    |
| **Count-Min**     | "How many times did this occur?" | Redis Stack | No                 | Overestimates, never underestimates |
| **Top-K**         | "What are the top N items?"      | Redis Stack | No                 | Time-decaying HeavyKeeper           |
| **t-digest**      | "What is the 99th percentile?"   | Redis Stack | No                 | Higher accuracy at the tails        |

#### Indexing
- Indexing in Redis transforms the database from a simple key-value store where you must know the exact key to fetch data, into a queryable database by creating secondary structures that sit alongside your data. Without an index, finding records matching specific criteria requires iterating through the entire keyspace using commands like `SCAN`, which is an O(N) operation and far too slow for real-time application request paths.
- By defining an index schema, you tell Redis to watch keys matching a specific prefix and automatically build a searchable structure from their fields. This allows for complex operations—like full-text search, numeric range queries, aggregations, and vector similarity searches—to execute with extreme low latency.
- **Field Types & Their Purposes:**
	- **TEXT:** Used for full-text search. Redis tokenizes the content and applies stemming (e.g., searching "running" matches "run") to build an inverted index.
	- **TAG:** Used for exact-match filtering. Unlike TEXT, TAG fields are treated as atomic units and are not tokenized or stemmed. They are ideal for categories, statuses, or IDs.
	- **NUMERIC:** Supports range queries (greater than, less than) and sorting results.
	- **GEO:** Enables location-based queries, such as finding documents within a specific radius or bounding box.
	- **VECTOR:** Stores embeddings for semantic search. You must know the two algorithms: **FLAT** (brute-force, exact results, linear scaling time) and **HNSW** (graph-based, approximate nearest neighbors, highly scalable).
- **JSON vs. Hash Indexing Behavior:**
	- When storing arrays of tags in a Hash, a comma-separated string like `"black,silver"` defaults to creating two distinct tags: `"black"` and `"silver"`.
	- When storing the same string in a JSON document without explicitly defining a separator in the index schema, it becomes a single tag: `"black,silver"`. To split it, you must define the separator `","` in the index.
	- If a JSONPath expression targets multiple values, string and numerical values are indexed, `null` values are skipped, and any other data type causes an indexing failure.
- **Indexing Timing:** New or modified documents are indexed synchronously (available immediately upon command completion), whereas existing documents in the database at the time of index creation are scanned and indexed asynchronously in the background.


# Exam Notes

## 1. Data modeling with Redis Data Structures
### 1.1 Data Structures
- Strings
	- Basic Redis type
		- Binary Safe
		- Hold any data (text, serialized objects, JPEGs, Integers)
		- Max Size 512 MB
		- SET, GET, INCR - O(1) Time complexity
		- Use cases
			- Caching HTML / API response
			- Session management
			- Atomic Counters
		- Jedis Commands
			- jedis.set()
			- jedis.get()
			- jedis.incr()
		- Good for fetching or overwriting entire payload at once. atomic counters
		- Bad for reading or updating a single field in a massive 500 KB object (requiring a network transfer of a whole object)
- Hashes
	- Its field value pair inside a redis key
		- optimized to represent objects
		- Store up to 2^32 - 1 ~ 4.29 billion fields
		- HGET, HSET in O(1)
		- HGETALL in O(N)
		- Use cases
			- User profiles
			- Application configuration
			- Rate-limiting counters
		- Java DS is Map<String, String>
		- Jedis Commands
			- jedis.hget()
			- jedis.hset()
			- jedis.hincrby()
		- Good for updating specific flat fields, rate limits
		- Bad for storing deeply nestad data or arrays
- Lists
	- LinkedList and not arrays / strings
		- Max 2^32 - 1 elements
		- LPUSH, RPUSH in O(1)
		- access by index LINDEX - O(N)
		- Use cases
			- message queues - producer consumer
			- activity streams
			- recent items lists
		- Good for sequential processing queues - head tail O(1)
		- Bad for random access or checking for existance O(N), if you need contains use sets
- Sets
	- Unordered collection of unique elements
		- Allow union and intersection across keys
		- Max 2 ^ 32 - 1 elements
		- SADD, SISMEMBER in O(1)
		- SINTER, SUNION depends on size of sets
		- Use cases
			- Tracking unique IP addresses
			- Tagging systems
			- relationship mapping
		- Faster membership checks, deduplication, finding intersections
		- bad for maintaining orders or fetching item by index
- Sorted Sets / Zsets
	- Sorted strings hold unique strings, but every string is associated with a floating point score.
		- elements are always sorted / kept by this score
		- ZADD is O(logN)
		- ZRANGE is O(logN + M), M is no. of elements used
		- Use cases
			- Leaderboards
			- Priority queues
			- Time series data using UNIX timestamp as a score
	- Range queries by score - ZRANGE
	- Bad for unordered collections
- JSON
	- Redis stack as part of RedisJSON
		- Native JSON documents allowing partial updates and fast queries
		- JSONPath syntax
		```
		$ for root
		$.name for field
		```
		- Reading and writing deep path is substantially faster and uses less network bandwidth than entire document read, update and write
		- Use cases
			- String complex nested documents
			- Product catalogs
			- User configurations
		- JSONDoc
		- JSONGet
		- JSONSet
	- Deeply nested document queries, path level updates, appending to arrays
	- Bad for simple KV look ups (unnecessary parsing)

- Hashes vs. Strings for Record storage ?
	- Strings
		- Serialized string approach best for read all / write all
		- problem is concurrency / bandwidth
		- best for HTML rendering & finding database query results
	- Hashes
		- Best for partial updates
		- Its use case is login_count or last_active timestamp frequency updates then hashes are superior with HINCRBY or HSET command
		- Memory & serialization is a tradeoff
- Hash vs. JSON for structural data ?
	- Hashes are strictly one dimentional
	- Redis JSON natively supports nested objects and arrays
	- JSON uses path access support
	- JSON handles this server side but for hash we need to pull in client and manage
	- Atomic field operations both support
- Choose the structures based on access patterns
	- strings vs. hash vs. JSON based on use case


### 1.2 Model and manipulate records of Hashes

- HSET semantics &  return values
	- HSET merges new data into the existing hash
	- It does not erase existing fields that are missing from your current command
	- HSET returns an integer representing the number of new fields created
		- If you add brand new - 1
		- existing update - 0
	- return 0 does not mean command fails, it means existing field updated
	- HSET accepts multiple field-value pair in a single command
- Missing keys & fields
	- No exception only empty response
	- missing field HGET returns nil, null in java
	- missing key - HGET - nil, HMGET - empty map, HGETALL - empty list in Java
	- No need for exists call
- Atomic and conditional operations
	- HSETNX - set if not exists 
		- set only if not exist if already there then returns 0 without doing anything
	- HINCRBY - Increment numeric field by a specific value
	- Always use HINCRBY for thread safety and never do HGET & HSET
- HDEL on field deletes root if no other field in a hash by key
- Predicting a state after partial updates
	- Implementing counters and stock decrements
	- To manage inventory & rate limits safety you must rely on atomic commands
	- There is no HDECRBY but we should use HINCRBY with negative value
	- HINCRBY gives find value by operation so it will help us validate & there won't be additional need to read again
- Avoid full records replacement when only few fields are updated
	- Bad pattern to do HGETALL - map to Java object - update avatar - HSET instead
	- do HSET user:1 avatar "new_image.jpg"

### 1.3 Work With JSON documents

- RedisJSON / Redis Stack
- JSONPath syntax
	- $ - represents top most root of a document for doc {} or \[] array
	- . - dot notation for nested objects
	- \[] - backets for arrays or key with spaces
	- e.g.
		- $.user.name
		- $.skills\[0]
		- $.\['last name']
- Wild card - *
	- select all elements at a specific level
	- e.g
		- $.uses\[\*].name - returns a collection of all names within the users array or objects
- Recursive decent - .. (two dots)
	- Deep search
	- $..name will find every key named name any where in the entire document, regardless of depth.
- Filter expression - \[?(@.condition)] 
	- Used to select array elements based on Criteria rather than index.
	- @ represents current element being processed
	- Ex. $.inventory\[?(@.price < 50)] - returns all inventory items costing less than 50
- How JSONPath queries returns results
	- Any JSON path query that could return multiple results always returns an array of values, even if only one match is found
	- If you query specific, explicit path $.user.name it returns the single value directly
	- If you query $.user\[\*].name and there is only one user. it returns \["Alice"] not "Alice".
- Core JSON operations
	- JSON.SET
		- fundamental write command
		- JSON.SET key $ '{"a":1}' overwrites the root
		- you can target specific path JSON.SET key $.a 2
	- JSON.MERGE
		- Deeply manages new JSON object into an existing one
		- Additive - new keys are added
		- Overwriting - existing keys are updated with new values
		- NULL deletion
			- If you pass null as the value for a key in JSON.MERGE payload, that key is deleted from the document
	- JSON.DEL
		- Deletes a value at a specific path
		- JSON.DEL key $.password removes a password field.
		- if called at the root JSON.DEL key $ - it deletes the entire key
	- Array Operations
		- Array operations without fetching the document
			- JSON.ARRAPPEND key $.skills "Redis" - adds at the end
			- JSON.ARRINSERT key $.skills 0 "Java" - adds at the specific index
			- JSON.ARRPOP key $.skills -1 
				- removes and returns an element 
				- default is the last element
- Reading nested values across arrays with wild cards
- Updating a single nested value in a place
	- You must avoid the anti pattern of fetching the document to change one piece of data
- Order-Resilient updates using filter expression
	- Never depend on indexes to update, always prefer filter expression
- Multi-field updates with JSON.MERGE
	- E.g.
	```
	JSON.MERGE user:1 $ '{"email": "new@gmail.com", "phone": "999-888-777", "secondary_address": null}'
	```
	- update the email, adds phone and deletes the secondary_address field

### 1.4 Collections
- Lists - Linked list
	- head / tail operations are fast but index look up are slow
	- LPUSH - head push (left)
	- RPUSH - tail push (right)
	- pushing multiple elements in one command reverses the append order e.g.
		- LPUSH mylist A B C results into C B A
	- reading a missing list by key returns nil
	- pushing in missing key creates a list
	- LTRIM 
		- LPUSH followed by LTRIM to maintain a capped feed
		- LTRIM key start end - keeps elements in provided range and deletes rest. 0 based indexing
	- LTRIM key 0 99 - keeps first 100 elements negative indexes supported, -1 is the last element
- Sets - Unordered, unique
	- Mathematical structures
	- SINTER, SUNION vs. pulling the data to the client
	- SADD returns value, cnt of newly added elements, if no new 0.
	- Uniqueness guaranteed
	- SINTER - intersection
	- SUNION - union
	- SDIFF - Difference returns element present in the first set but not in any subsequent sets (order matters here)
	- Commands like SINTERSTORE, SUNIONSTORE stores results in specified new key instead of returning result to the client
- Sorted Set - ZSet
	- Uniqueness + score ordered floating number
	- If scores are tied, elements are ordered lexicographically
	- ZADD - update vs. insert 
		- updates score based on existing update
		- returns the no. of new members added
	- ZINCRBY - Auto creation
		- initial score with 0 if not exist then increases
	- ZRANK - direction
		- Returns the index (0 - based) of a member sorted from low to high score.
		- for leaderboards where highest in 1st place, you must use ZREVRANK
- Range Queries
	- Index vs. Score
		- ZRANGE key 0 9 - top 10 elements
		- ZRANGEBYSCORE key 100 200 - all elements with score between 100 and 200
	- WITHSCORES to get member with scores in RANGE command we must pass WITHSCORES flag
	- Automatic key deletion
- When the last element or field is removed from a collection, Redis automatically deletes the key

- Implementing command patterns
	- Capped Activity feeds - Use lists (LPUSH + LTRIM)
	- Uniqueness tracking - Use sets
	- Leaderboards - Use sorted sets
- Choose right set operations
	- Membership overlap - SINTER
	- Combining categories without duplication - SUNION
	- SetA - SetB - SDIFF
- Always do server side aggregation and minimize network I/O

### 1.5 Key Namespaces
- Hierarchical key naming with colon separators.
- Redis is a flat-key store
- It has no table, schemas or namespaces
- To create structure, developer uses colon : as conventional based separators
- Standard pattern
	- object_type:id:attribute
	- Ex.
		- user:101:profile
		- user:101:cart
- Your key schema dictates how efficiently you can search for keys using the SCAN command
- SCAN is preferred over KEYS to avoid blocking server
- Effective Scaning:
	- SCAN 0 MATCH user:\*:pattern
- keys are binary safe, it can be anything like empty string, readable string, serialized java object or an image file
- Always prefer UTF-8 strings for key
- maximum size for a key is 512 MB
- Memory & Bandwidth overhead for huge keys

- Design consistent key schemas
- Avoid flat ambiguous or over-compressed names
- Reduce top level keys and group related data
- e.g
	- user:101:name, user:101:age, user:101:email instead of this do user:101 as hash with fields name, age and email

