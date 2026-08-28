
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
