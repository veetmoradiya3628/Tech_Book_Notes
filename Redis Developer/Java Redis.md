
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

