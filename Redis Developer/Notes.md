
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

