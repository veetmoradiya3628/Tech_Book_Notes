
## Transactional
- Critical section
	- code segment, where shared resources are being accessed and modified
	- When multiple request try to access this critical section, Data inconsistency can happen
	- Its solution is usage of Transaction
- It helps to achieve ACID
	- Atomicity
	- Consistency
	- Isolation
	- Durability

```
BEING_TRANSACTION:
	Debit from A
	Credit to B
	if all success:
		COMMIT;
	else
		ROLLBACK;
END_TRANSACTION;
```

- In spring boot we can use `@Transactional` annotation
- dependency
	- `spring-boot-starter-data-jpa`
- Activate, Transaction Management by using @EnableTransactionManagement in main class
- It can be applied at 2 level
	- At class level
		- transaction applies to all public methods
	- At method level
		- transaction applied to particular method only

- Transaction management in spring boot uses AOP
	- Uses point cut expression to search for method, which has @Transactional annotation like
		- @within(org.springframework.transaction.annotation.Transactional)
	- Once point cut expression matches, run an "Around" type Advice
		- Advice is **invokeWithinTransaction** method present in **TransactionalInterceptor** class

![[TransactionHierarchy.png]]

- Transaction management 2 ways
	- Declarative
		- Transaction management via annotations
	- Programmatic
		- Transaction management through code
		- flexible but difficult to manage
		- 2 ways
			- @Bean registration config
			- Using Transaction template
- Propagation
	- When we try to create a new transaction, it first check the PROPAGATION value set, and this tell whether we have to create new transaction or not
		- REQUIRED - Default propogation
			- if parent txn present then use it else create new transaction
		- REQUIRED_NEW 
			- if parent txn present suspend it & create new transaction and once finished, resume the parent txn else create new txn and execute the method
		- SUPPORTS
			- if parent txn exists then user it else run method without tnx
		- NOT_SUPPORTED
			- if parent txn exist then suspend it execute the method without any transaction and resume the parent txn else execute the method without any transaction
		- MANDATORY
		- NEVER
- Isolation level
	- It tells how the changes made by one transaction are visible to other transaction running in parallel

![[Pasted image 20260725131720.png]]
- Dirty Read Problem
	- Transaction A reads the un-committed data of other transaction and if other transaction is rolled back, the un-committed data which is read by Transaction A is known as Dirty Read
- Non-Repeatable Read problem
	- If suppose transaction A reads the same row several times and there is a chance that it get  different value, then its known as Non-Repeatable Read problem
- Phantom Read problem
	- If suppose transaction A executes same query several times, but there is a chance that rows returned are different
- DB locking types
	- Shared lock known as READ lock
	- Exclusive lock known as WRITE lock

![[Pasted image 20260725132232.png]]

## Async
- ThreadPool
	- It's collection of threads (aks Workers) which are available to perform the submitted tasks.
	- Once task completed, worker thread get back to Thread pool and wait for new task to assigned
	- Means threads can be reused
- In Java Thread pool is created using ThreadPoolExecutor Object
- Async Annotation
	- Used to mark method that should run asynchronously
	- Runs in a new thread, without blocking the main thread
- Spring boot first looks for **defaultExecutor** if no defaultExecutor found only then **SimpleAsyncTaskExecutor** is used
- ThreadPoolTaskExecutor is nothing but a Spring boot Object, which is just a wrapper around Java ThreadPoolExecutor
	- Its not recommanded
		- Underutilization of Threads
		- High Latency
		- Thread Exhaustion
		- High memory Usage
- Create our own custom, ThreadPoolTaskExecutor
	- During application startup, spring boot sees that ThreadPoolTaskExecutor Bean present so it makes it default only
	- And even when we use @Async without any name, our custom thread pool executor will get picked only
	- recommended approach
- Creating our own custom, ThreadPoolExecutor (java one)
	- Not recommended because
		- Thread Exhaustion
		- Thread creation overhead
		- High memory usage
- Condition for @Async
	- The `@Async` annotation must be applied to a method in a different class than the caller.
	- If the method is called from within the same class, the proxy mechanism is skipped because internal method calls are not intercepted.
	- A method annotated with `@Async` must be public.
	- This public visibility is required because Aspect-Oriented Programming (AOP) interception works only on public methods.
- @Async and Transaction management
	- Transaction Context does not transfer from the caller thread to the new thread created by the `@Async` execution.
	- When a new thread is created, it will have its own transaction management, but this context is separate from the parent thread.
	- Because the context differs between threads, transaction propagation will not work as expected.
- Method return types
	- Both `Future` and `CompletableFuture` can be used as the return type for an `@Async` method.
- Exception Handling
	- Exceptions can be caught using a standard `try-catch` block when calling the `.get()` method on the result object.
	- If the `@Async` method has a `void` return type, exceptions will not propagate back to the calling thread for a standard `try-catch`.
	- For `void` methods, you can handle the exception using a `try-catch` block directly within the `@Async` method itself.
	- If a `void` method throws an exception and it is not explicitly handled, Spring Boot's default `SimpleAsyncUncaughtExceptionHandler` will be invoked, logging an "Unexpected exception occurred invoking async method" error.

- Interceptors
	- it's mediator which get invoked before or after your actual code

![[Pasted image 20260725190738.png|695]]
- Interface `HandlerInterceptor`
	- methods preHandle, postHandle, afterCompletion
- `addInterceptors` methods to add methods

- Custom Interceptor for Requests after reaching to specific Controller class
	- Create custom annotation
	- Target, Retention etc
- Filter
	- It intercept the HTTP Request and Response, before they reach to the servlet
	- We can have many filters and have ordering between them too
- Interceptor
	- Its specific to Spring Framework, and intercept HTTP Request and Response, before they reach to the controller
	- We can have many Interceptor and have ordering between them too

![[Pasted image 20260725194129.png]]

- Interceptor implementation
	- implement `WebMvcConfigurer` & override `addInterceptors` to add interceptor

- HATEOS
	- Hypermedia As The Engine Of Application State
	- It tells the client, what the next action you can perform on particular item
	- Purpose
		- Loose coupling 
		- API Discovery

![[Pasted image 20260725202958.png]]

- will add required net set of possible actions in the response

![[Pasted image 20260725203044.png]]
- Dependency :- `spring-boot-starter-hateoas`

- Response Entity & Codes
	- Response contains
		- Status Code
		- Header
		- Body
	- We can use ResponseEntity\<T>  to create Response and in this 'T' represents the type of the Body
	- build method can be used to response to API
- @ResponseBody
	- When we return POJO in response we need @ResponseBody annotation is required
	- @RestController automatically puts @ResponseBody to all the methods
	- for @Controller it needs to be annotated with @ResponseBody else it will throw an exception because it will try to find the file with response string name
- Response Codes
	- 1xx
		- Informational
	- 2xx
		- Success
	- 3xx 
		- Redirection
	- 4xx
		- Validation error
	- 5xx
		- Server Error
- 2xx
	- 200 - OK
	- 201 - Created
	- 202 - Accepted
	- 204 - No Content
	- 206 - Partial Content
- 3xx
	- 301 - moved permanently
	- 308 - Permanent redirection
	- 304 - Not modified
- 4xx
	- 400 - Bad request
	- 401 - Unauthorized
	- 403 - Forbidden
	- 404 - Not Found
	- 405 - Method not allowed
	- 422 - Un-processable entity
	- 429 - Too many requests
- 5xx
	- 500 - Internal Server Error
	- 501 - Not Implemented
	- 502 - Bad Gateway

- Exception Handling

![[Pasted image 20260726140349.png]]

- When an exception occurs, the `DispatcherServlet` intercepts it and passes it to the `HandlerExceptionResolverComposite`
- The composite executes a left-to-right flow through specific resolvers: first `ExceptionHandlerExceptionResolver`, then `ResponseStatusExceptionResolver`, and finally `DefaultHandlerExceptionResolver`
- Each resolver attempts to set the proper HTTP status and message for the exceptions it is responsible for handling
- If a custom `ResponseEntity` object is not created by the developer (e.g., just throwing the exception), the exception passes through these resolvers without a final body being built
- When the resolvers do not build a full response, control reaches the `DefaultErrorAttributes` class
- `DefaultErrorAttributes` is responsible for filling the HTTP response with default values, returning standard error attributes like timestamp, status, error name, and path.
	- ExceptionHandlerExceptionResolver
	- ResponseStatusExceptionResolver
	- DefaultHandlerExceptionResolver