
JDBC
- JDBC - Java Database Connectivity provides an interface to
	- Make connection with DB
	- Query DB
	- and process the result
- Actual implementation is provided by Specific DB Drivers
	-  MySQL
		- Driver: Connector/J
		- Class: com.mysql.cj.jdbc.Driver
	- PostgreSQL
		- Driver: PostgreSQL JDBC Driver
		- Class: org.postgresql.Driver
	- H2 (in-memory)
		- Driver: H2 Database Engine
		- Class: org.h2.Driver
- Connection, Statement, PreparedStatement, ResultSet etc
- Lot of boiler plate code

- JDBC with spring boot
- `spring-boot-starter-jdbc`
- Spring boot provides `JDBCTemplate` class, which helps to remove all the boiler code
- Driver class loading
- DB Connection Making
- Exception Handling
- Closing of the DB connection and other resources
- Manual handling of DB Connection pool
- methods
	- update
	- query
	- queryForList
	- queryForObject

JPA Architecture

![[Pasted image 20260727082619.png]]

ORM - Object-Relation Mapping
- Acts as a bridge between Java Object and Database tables
- Unlike JDBC, where we have to work with SQL, with this we can interact with database using Java Objects
- dependency `spring-boot-starter-data-jpa`
- application properties like datasource url, driver class name, username and password needs to be setup

Internal Architecture

![[Pasted image 20260727082904.png]]

- Persistent Unit
	- Logical group of Entity classes which share same configurations
	- Configuration details like
		- Database connection properties
		- JPA provider (hibernate etc) etc
- EntityManagerFactory
	- Using Persistent unit configuration, entity manager factory object get created during application startup
	- If any property is not found / provided, default one is picked and set
	- 1 EntityManagerFactory for 1 Persistent Unit
	- This class act as a Factory to create an Object of Entity Manager
- Transaction manager association with EntityManagerFactory
	- During persistence unit, we have specified the value of "transaction-type" value either:
		- RESOURCE_LOCAL (default)
		- JTA (Java Transaction API)
	- 2 types
		- Manager which manages transaction for 1 DB
		- Manager which manages transaction which can span across multiple DB, That's also possible using JTA
	- During Application Startup after EntityManagerFactory object created, based on RESOURCE_LOCAL or JTA, transaction manager object get created
	- we can configure by our own by overriding transactionManager bean
- EntityManager and Persistence Context
	- EntityManager
		- Its an Interface in JPA that provides methods to perform CRUD operations on entities
			- persist() - for saving
			- merge() - for updating
			- find() - for fetching
			- remove() - for deleting
			- createQuery() - for executing JPQL queries
		- EntityManager interface methods are implemented by JPA vendors like Hibernate etc
		- EntityManagerFactory helps to create an Object of EntityManager
	- PersistentContext
		- Consider it a first level cache
		- for each EntityManager, PersistentContext object is created, which hold list of Entities its working on
		- Also manage the life cycle of entity
	- EntityManager Insert/Update/Delete operations are Transaction bounded, means it first checks if `Transaction` is open, if not it will throw Exception
	- but not all (read operations are not transaction bounded)
- Internally JPA repository all insert, update, delete methods are annotated with @Transactional so even if we do not write, spring framework takes care of it
- Life cycle of Entity in persistenceContext

![[Pasted image 20260727084935.png]]
- First Level Caching
	- During application startup JPA creates fresh DB and tables
	- When we do insert and read then only insert will run
	- PersistentContext is associated with EntityManager
	- And EntityManager is created for each HTTP Request, so different methods within same HTTP Request share the EntityManager
	- save() & findById() - internally both uses the same entity manager, therefore when findById() invoked, cache hit will happen
- Second Level Caching

![[Pasted image 20260727090502.png]]
- few application properties & few dependency in pom.xml needs to be added
- Dependencies
	- org.ehcache - provides the core implementation of Second level caching
	- hibernate-jcache - Hibernate specific caching logic comes with this, like we use annotations over entity @Cache, we used CacheConcurrenyStrategy so specific logic need to be executed, and this library helps us with that
	- javax.cache - cache-api - Provides interface for JCache, hibernate interacts with this APIs.

![[Pasted image 20260727090924.png]]

- Region
	- Helps in logical grouping of cached data
	- For each region we can apply different caching strategy like
		- Eviction policy
		- TTL
		- Cache Size
		- Concurrency strategy etc
	- Which helps in achieving granular level management of cached data (either Entity, Collection, or Query results)
- CacheConcurrencyStrategy
	- READ_ONLY
		- Good for static data
		- which do not require any updates
		- if try to update just entity, exception will come
	- READ_WRITE
		- During Read, it puts shared lock, other read can also acquire shared lock, but no write operation
		- During update, it put exclusive lock, other Read and Write operation not allowed
	- NONSTRICT_READ_WRITE
		- During READ, No lock is acquired at all
		- During update, after txn commit successful, cache is mark invalidated and not updated with fresh data
		- good for heavy read application
		- so if update and read happens in parallel, it's chance that read operation get the stale data
	- TRANSACTIONAL
		- Acquire READ lock and also WRITE lock
		- Updates the cache too, after tnx commit successfully
		- any other read operation during cache lock, goes directly to DB
		- any other write operation during cache lock, waits in queue

- DTO - table
- `spring.jpa.hibernate.ddl-auto` configuration tells hibernate regarding how to create and manage the DB schema

![[Pasted image 20260727093237.png]]

- Mapping classes to Tables
	- @Table annotation
		- optional field
		- Generally follows camel case to upper case means userDetails - USER_DETAILS
		- configuration options
			- name
			- schema
			- uniqueConstraints
			- indexes
	- @Column annotation
		- Its an optional field, if not defined, JPA will add it with default values
	- @Id
		- Primary Key, must be unique
		- Each entity can have only 1 primary key
		- Only 1 field can be annotated with @Id
		- combination of two or more columns to form a primary key
			- using @Embeddable and @EmbeddedId annotation
			- using @IdClass and @Id annotation
	- @GeneratedValue
		- Primary Key generation strategy
		- IDENTITY
			- Each insert, generates a new identifier (auto-increment field)
		- SEQUENCE
			- Used to generate unique numbers
			- speed up the efficiency when we cache sequence values
			- More control than IDENTITY
			- @SequenceGenerator has field like
				- name
				- sequenceName
				- initialValue
				- allocationSize
		- TABLE
			- Separate table for managing the sequence which is really inefficient

- @OneToOne Unidirectional
	- One Entity (A) references only one instance of another Entity (B)
	- But reference exist only in one Direction. i.e. from Parent(A) to Child(B)
	- By default hibernate choose 
		- the FK name as <field_name_id>
		- chooses the primary key (PK) of other table
	- But if we need more control over it, we can use @JoinColumn annotation
	- We can use @JoinColumns to need to map composite primary key
- CASCADE Type
	- Without cascadetype, any operation on Parent do not affect child entity, managing child entities explicitly can be error-prone
	- ALL
	- PERSIST
		- Persisting / Inserting the User entity automatically persists its associated UserAddress entity data
	- MERGE
		- Updating the user entity automatically updates its associated UserAddress entity data
	- REMOVE
		- Deleting the User entity automatically delete its associated UserAddress entity data
	- REFRESH
		- It should not read child data from first level caching, instead we mark JPA to configure it to read child from directly database and not from cache
	- DETACH
		- To remove from persistent context we tell JPA to remove associated child entity entries as well

- Get data
	- @Eager loading
		- It means associated child entity is loaded immediately along with the parent entity
		- default for @OneToOne and @ManyToOne
	- @Lazy loading
		- It means, associated entity is not loaded immediately, only loaded when explicitly accessed like we call userDetail.getUserAddress()
		- Default for @OneToMany, @ManyToMany
	- We can configure like FetchType configuration with annotation

- Get operation serialization issue fix in case of Lazy / Eager evaluation
	- Use @JsonIgnore
		- This will remove the UserAddress field totally for both Lazy and Eager Loading
	- Using DTO (Data transfer object)
		- Much cleaner and recommended approach
		- Instead of sending Entity directly, first response will be mapped to our DTO object

- OneToOne - BiDirectional
	- Both entities holds reference to each other means:
		- UserDetails has a reference to UserAddress
		- UserAddress also has a reference back to UserDetails (only in Object, not in DB table)
	- From table side it looks exactly same but now we have capability to go backward in the owner object from inverse side
	- On Serialization response it leads to exception due to recursive detection between entities to solve this
		- @JsonManagedReference
			- Should be used only in Owing entity
			- Tells explicitly Jackson to go ahead and serialize the child entity
		- @JsonBackReference
			- Should only be used with Inverse/Child entity
			- Tells explicitly Jackson to not serialize the parent entity
	- To load entity from both the ends but avoiding infinite recursion
		- @JsonIdentityInfo
			- During serialization, Jackson gives the unique ID to the entity (based on property field)
			- Through which Jackson can know, if the particular id entity is already serialized before, then it skip the serialization

- One-to-Many (Unidirectional)
	- One entity associated with multiple records in another entity
		- Like user can have many orders
	- Reference exist in only 1 direction, i.e. from Parent to child
	- Since its 1 to Mapping so it creates new table and stores the mapping
	- By default its Lazy loading, means when query parents, child rows are not fetched
	- If we don't want to create new table then we can use @JoinColumn this also tells JPA that we want to store the FK in child table instead of creating a new table
	- Lazy & Eager fetch works as above
	- Casecade type supported
		- PERSIST
		- MERGE
		- REMOVE
		- ALL
	- Orphan removal
		- automatically removes child entry when child removed from parent collection
- One-to-many (Bidirectional)
	- Parent reference to child
	- Each child reference to parent
- Many-to-One (Unidirectional)
	- we talks from child perspective
- Many-to-many (Unidirectional)
	- Reference from one way only
	- Join table must
- Many-to-many (Bidirectional)
	- Since its many to many, anyone can be owing and inverse side

