
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

