
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