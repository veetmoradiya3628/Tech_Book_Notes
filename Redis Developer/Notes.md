
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


- Redis data structures
	- Strings - text
	- Bitmaps - Bitmap encoding
	- Bit field - Efficient integers
	- Hashes - Sessions / profiles
	- Lists - Queues
	- Sets - Recommendations
	- Sorted sets - Leaderboards
	- Geospatial indexes - Location services
	- Hyperlog-log - statistical estimations
	- Streams - Event stream processing