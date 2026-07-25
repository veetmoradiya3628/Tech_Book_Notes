
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
