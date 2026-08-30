
## 1. General Key Commands

| Jedis Command                | Redis Equivalent | Description                                        | Time Complexity                                                                          |
| :--------------------------- | :--------------- | :------------------------------------------------- | :--------------------------------------------------------------------------------------- |
| `jedis.exists(key)`          | `EXISTS`         | Checks if a key exists in the database.            | **O(1)**                                                                                 |
| `jedis.del(key...)`          | `DEL`            | Deletes one or more keys.                          | **O(N)** where N is the number of keys.                                                  |
| `jedis.expire(key, seconds)` | `EXPIRE`         | Sets a timeout on key.                             | **O(1)**                                                                                 |
| `jedis.ttl(key)`             | `TTL`            | Returns the remaining time to live of a key.       | **O(1)**                                                                                 |
| `jedis.keys(pattern)`        | `KEYS`           | Finds all keys matching the given pattern.         | **O(N)** where N is the total number of keys in the database.<br>*(Avoid in production)* |
| `jedis.type(key)`            | `TYPE`           | Returns the data structure type stored at the key. | **O(1)**                                                                                 |

## 2. String Commands

| Jedis Command                      | Redis Equivalent | Description                                   | Time Complexity                                     |
| :--------------------------------- | :--------------- | :-------------------------------------------- | :-------------------------------------------------- |
| `jedis.set(key, value)`            | `SET`            | Sets the string value of a key.               | **O(1)**                                            |
| `jedis.get(key)`                   | `GET`            | Gets the value of a key.                      | **O(1)**                                            |
| `jedis.setex(key, seconds, value)` | `SETEX`          | Sets the value and expiration of a key.       | **O(1)**                                            |
| `jedis.incr(key)`                  | `INCR`           | Increments the integer value of a key by one. | **O(1)**                                            |
| `jedis.decr(key)`                  | `DECR`           | Decrements the integer value of a key by one. | **O(1)**                                            |
| `jedis.mset(key1, val1...)`        | `MSET`           | Sets multiple keys to multiple values.        | **O(N)** where N is the number of keys to set.      |
| `jedis.mget(key1, key2...)`        | `MGET`           | Gets the values of all the given keys.        | **O(N)** where N is the number of keys to retrieve. |

## 3. Hash Commands

| Jedis Command                   | Redis Equivalent | Description                               | Time Complexity                                         |
| :------------------------------ | :--------------- | :---------------------------------------- | :------------------------------------------------------ |
| `jedis.hset(key, field, value)` | `HSET`           | Sets the string value of a hash field.    | **O(1)** for each field/value pair added.               |
| `jedis.hget(key, field)`        | `HGET`           | Gets the value of a hash field.           | **O(1)**                                                |
| `jedis.hgetAll(key)`            | `HGETALL`        | Gets all the fields and values in a hash. | **O(N)** where N is the size of the hash.               |
| `jedis.hdel(key, field...)`     | `HDEL`           | Deletes one or more hash fields.          | **O(N)** where N is the number of fields to be removed. |
| `jedis.hexists(key, field)`     | `HEXISTS`        | Determines if a hash field exists.        | **O(1)**                                                |
| `jedis.hkeys(key)`              | `HKEYS`          | Gets all the fields in a hash.            | **O(N)** where N is the size of the hash.               |

## 4. List Commands

| Jedis Command                   | Redis Equivalent | Description                                   | Time Complexity                                                                   |
| :------------------------------ | :--------------- | :-------------------------------------------- | :-------------------------------------------------------------------------------- |
| `jedis.lpush(key, value...)`    | `LPUSH`          | Prepends one or multiple values to a list.    | **O(1)** for each element added.                                                  |
| `jedis.rpush(key, value...)`    | `RPUSH`          | Appends one or multiple values to a list.     | **O(1)** for each element added.                                                  |
| `jedis.lpop(key)`               | `LPOP`           | Removes and gets the first element in a list. | **O(1)**                                                                          |
| `jedis.rpop(key)`               | `RPOP`           | Removes and gets the last element in a list.  | **O(1)**                                                                          |
| `jedis.lrange(key, start, end)` | `LRANGE`         | Gets a range of elements from a list.         | **O(S+N)** where S is distance of start offset, N is number of elements in range. |
| `jedis.llen(key)`               | `LLEN`           | Gets the length of a list.                    | **O(1)**                                                                          |

## 5. Set Commands

| Jedis Command                  | Redis Equivalent | Description                                       | Time Complexity                                          |
| :----------------------------- | :--------------- | :------------------------------------------------ | :------------------------------------------------------- |
| `jedis.sadd(key, member...)`   | `SADD`           | Adds one or more members to a set.                | **O(1)** for each element added.                         |
| `jedis.smembers(key)`          | `SMEMBERS`       | Gets all the members in a set.                    | **O(N)** where N is the set cardinality.                 |
| `jedis.srem(key, member...)`   | `SREM`           | Removes one or more members from a set.           | **O(N)** where N is the number of members to be removed. |
| `jedis.sismember(key, member)` | `SISMEMBER`      | Determines if a given value is a member of a set. | **O(1)**                                                 |
| `jedis.scard(key)`             | `SCARD`          | Gets the number of members in a set.              | **O(1)**                                                 |

## 6. Sorted Set (ZSet) Commands

| Jedis Command                    | Redis Equivalent | Description                                                                         | Time Complexity                                                                                                   |
| :------------------------------- | :--------------- | :---------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------- |
| `jedis.zadd(key, score, member)` | `ZADD`           | Adds one or more members to a sorted set, or update its score if it already exists. | **O(log(N))** for each item added, where N is the number of elements in the sorted set.                           |
| `jedis.zrange(key, start, end)`  | `ZRANGE`         | Returns a range of members in a sorted set, by index.                               | **O(log(N)+M)** with N being the number of elements in the sorted set and M the number of elements returned.      |
| `jedis.zrem(key, member...)`     | `ZREM`           | Removes one or more members from a sorted set.                                      | **O(M*log(N))** with N being the number of elements in the sorted set and M the number of elements to be removed. |
| `jedis.zscore(key, member)`      | `ZSCORE`         | Gets the score associated with the given member in a sorted set.                    | **O(1)**                                                                                                          |
| `jedis.zcard(key)`               | `ZCARD`          | Gets the number of members in a sorted set.                                         | **O(1)**                                                                                                          |

## 7. Bitmap Commands

| Jedis Command                    | Redis Equivalent | Description                                                         | Time Complexity |
| :------------------------------- | :--------------- | :------------------------------------------------------------------ | :-------------- |
| `jedis.setbit(key, offset, val)` | `SETBIT`         | Sets or clears the bit at offset in the string value stored at key. | **O(1)**        |
| `jedis.getbit(key, offset)`      | `GETBIT`         | Returns the bit value at offset in the string value stored at key.  | **O(1)**        |
| `jedis.bitcount(key)`            | `BITCOUNT`       | Count the number of set bits (population counting) in a string.     | **O(N)**        |

---

### Important Notes on Time Complexity
*   **O(1) (Constant Time):** Extremely fast, execution time is always the same regardless of data size. Ideal for high-throughput operations.
*   **O(log N) (Logarithmic Time):** Very fast, execution time increases marginally as data grows. Common for Sorted Set (ZSet) operations.
*   **O(N) (Linear Time):** Execution time scales linearly with data size. 
	* **Caution:** O(N) operations like `KEYS *`, `HGETALL`, or `SMEMBERS` on very large datasets can block the Redis single thread, causing performance bottlenecks. Consider using pagination alternatives like `SCAN`, `HSCAN`, or `SSCAN` for large collections.