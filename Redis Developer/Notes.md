
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

