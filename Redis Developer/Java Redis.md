
- Setup

```
mvn clean install
```

- Jedis Notes
	- Simplicity
	- Principle of least astonishment
		- Redis SET - jedis.set
		- Redis GET - jedis.get
	- Concurrency and multi threading
		- Jedis() instances represent a single TCP socket, they are not thread-safe
		- Jetty is multi threaded
		- Jedis pool
	- Ex
	```
	String result = jedis.set("foo", "bar");
	
	// result will store "OK"
	```
- Redis clients
	- Manage connections
	- Implement Redis Serialization protocol
	- Provide a usable language-specific API
- Famous Java Redis clients
	- Jedis
	- lettuce
		- Need to write a non-blocking, reactive application but don't need distributed Java objects
	- Redisson
	- Choose the right tool for the job!
- Jedis Understanding
	- Jedis class vs. JedisPool 
	- Jedis instance are not thread-safe
	- JedisPool provides Jedis instance in thread safe manner
	- JedisPoolConfig
		- host
		- port
	- JedisSentinalPool
	- JedisCluster

![[Pasted image 20260825095647.png]]

- Basic Operations using Jedis

| Redis Type | Java Type            |
| ---------- | -------------------- |
| string     | String               |
| list       | List\<String>        |
| set        | Set\<String>         |
| hash       | Map\<String, String> |
| float      | Double               |
| integer    | Long                 |

- List, Set, Hash and its methods provided by Jedis for interacting with its internal data structure
- O(n) commands be careful with high-cardinality ds
	- LREM
	- SMEMBERS
	- Use SSCAN for high-cardinality sets
- Redis DAO design pattern
	- DAO
		- Data Access Object
		- Separates the data access interface from the logic for interacting with a given data store
		- Allows for multiple storage implementations
		- Domain objects are a separate concern
		- Domain objects
			- Pure data representations
		- DAO Interfaces
			- Data-store-agnostic API
		- DAO Implementations
			- Interact with a particular data store
- jedis.close() closes any and all sockets from the current JVM that are connected to Redis

- Storing meter reading metrics data in sorted set data structure will help
	- as it will help range query and multiple data in single key
	- key format depends on how we want to store the data
	- It will keep always in sorted fashion
	- Efficiency fetch a small range: O((logN) + m)
	- Efficient inserts - O(logN)
- jedis.zrevrangeWithScores() - method to get latest N recent elements

- Compare And Swap needs to be atomic for which Lua scripting can be useful
- To reduce too many round-trips to the server
	- Pipelining and transactions can be useful for this

- Lua Scripting with Jedis
	- Redis Lua scripts are like stored procedures
	- Execute custom logic on the server
	- Lua scripts execute atomically
- A real world app might use dozens of Lua scripts
- We need to keep scripts organized
- One class per script
- Load the script on initialization using the script load command
	- Cache the SHA
- Provide a usable Java interface
- Writing and organizing Lua scripts in Java

- Pipelining
	- Execute multiple commands in a single round trip
	- Efficient because
		- Reduces round-trip overhead
		- Reduces the number of syscalls
	- Read + Write both can be done in single 
	- Responses to all commands are returned at a once
	- `p.sync()`
	- `jedis.pipelined()`

- Transactions
	- Pipeline commands are not guaranteed to run as atomic
	- Jedis implements transaction as a pipeline
	- Efficient and atomic
	- `jedis.multi()`
	- `exec()`

- Use pipeline when
	- You have two or more commands to execute
	- Can wait for the responses of all commands at once
- Use a transaction if in addition
	- You require atomic execution of a set of commands

- JedisDataException - exception with transaction & pipeline

- Sorted Sets
- Geo
- Geo + Criteria + Lua
- Streams

- Leaderboard  keep track of rankings
- A common Redis use case
- Sorted set with score + element to implement leader board
	- score as capacity, member as siteId
- ZRANGE and ZREVRANGE for getting top N and bottom N elements out of the elements from sorted set
	- `zrangeWithScores method`
- One leaderboard is not enough in call the cases
	- so if needed we can create a leader board
		- city
		- region
	- Use key naming for scoping
		- e.g sites:capacity:ranking:\[city]
- ZRANGE is O((logN) + M)
- When sorted set has low cardinality, performance isn't a problem 
- With large sorted sets, retrieving large ranges may be expensive so keep your ranges small

- Geospatial
	- Store geospatial coordinates and issue queries against them
	- Under the hood:
		- Geohash
		- Sorted sets
	- GEOADD
	- GEORADIUS
	- With large sorted sets, consider ZSCAN for iterative retrieval
	- Optimize multiple HGETALL round trips with pipelining
- Streams
	- meter readings as metrics, stats and leaderboards
	- Redis streams can help
	- A data structure
	- Models an append-only log
	- Syndication for Redis streams (RU202 course)
	- XADD \[stream-name] \[ID] \[field-value pairs]
		- returns the ID of the stream element got added
	- XRANGE \[stream-name] + - COUNT 1
		- reads from oldest to newest
	- XREVRANGE
		- reads from newest to oldest
	- Streams are logically infinite, but redis servers don't have infinite memory
	- We need to control the length of a stream
	- XADD takes an optional argument, MAXLENGTH
	- Approximate length trimming gives a slight performance advantage
	- Java Map is use to represent a stream entry

- Rate limiting
- RedisTimeSeries
- Error Handling
- Connection Management
- Scaling
- Debugging
- Client Protocols

- Rate Limiting
	- Rate limiter keep track of the rate of user requests
	- Guard against careless and malicious users
	- Important for protecting server resources
	- Techniques
		- Fixed window
		- Sliding window
	- Fixed window rate limiter implementation using Redis + Jedis
	- `jedis.incr()` and `jedis.expire()` methods primarily can be used to expire the key and increase and counter for given minute
	- Key format depends on the structure as an

- Redis Time Series
	- Adds timeseries capability for Redis
	- sample of redis time series data consist of 
		- a timestamp in milliseconds
		- a value of type float/double
	- Tuple: a timestamp plus a measurement known as a sample
	- Range queries
	- Supports clean up of old samples with max retention settings
	- TS.CREATE \<key>
		- RETENTION option to keep how long recent data to keep in ms timing
		- CHUNK_SIZE 
		- DUPLICATE_POLICY - BLOCK /LAST/MAX/SUM etc
		- LABELS
	- TS.ADD  \<key> \<ts> or \* data
	- TS.ALTER 
	- TS.RANGE start & end time
		- Filter by value min max
	- TS.CREATERULE
	- JRedisTimesearies library for Java
- RedisSearch
	- full text capability
- RedisGraph
	- a graph database module for Redis


- Connection management
	- Client naming
	- Investing slow operations with SLOWLOG
	- Connection leaks
	- `CLIENT SETNAME <name>`
	- `CLIENT GETNAME`
	- `CLIENT LIST`
	- `SLOWLOG GET 1`
	- Client naming conventions
		- Hostname
		- Application name
		- ProcessID
	- default max total / max idle connection - 8
	- Deciding on max connections ?
		- how many threads does the app uses ?
			- one connection per thread for best performance
	- how many total connections ?
		- Redis does not allow for unlimited connections
		- Accepts 10,000 by default
- Error handling
	- On error, Jedis throws a `JedisException`
	- JedisException is a runtime exception
	- JedisConnectionException
	- JedisDataException
	- Jedis Cluster Exceptions

- Performance
	- Network latency
	- Time complexity
	- Atomicity and blocking
	- How do we minimize latency ?
		- Use pipelines when
			- you're running more than one command
			- you don't need intermediate responses
		- Pipelining reduces
			- Number of round trips
			- Context switching
			- Syscalls
	- Time complexities
		- know the time complexity of each and every command you use
		- constant - O(1)
		- logarithmic - O(log(N))
		- linear time command - O(N)
			- takes time to complete
			- redis is mostly single threaded
	- Atomicity and blocking
		- Transactions and Lua scripts block other commands
		- Consider the time complexity of commands run inside transactions and Lua scripts

- Debugging
	- Ways to debug the issue with Redis
		1. Check for expired keys
		2. Try it in CLI
		3. Monitor the commands being sent to Redis
		4. Special case: Lua
	- Monitor command in Redis
- Redis uses a RESP protocol
	- RESP2 is string human readable

Q. What is the use of `jedis.sismember()` ?

Reference material
- https://redis.io/tutorials/howtos/quick-start/cheat-sheet/


