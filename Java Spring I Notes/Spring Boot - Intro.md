
- Java Servlet
  - Servlets are standard
  - low level HTTP APIs to handle request & response
  - massive configuration file called `web.xml`
  - manually managing dependencies
  - package as war and deploy in standalone web server like Apache tomcat
  
- Spring Framework
  - built on top of the Servlet API but it routes all incoming requests through a single master Servlet called the DispatcherServlet
  - It introduces IoC (Inversion of Control) and DI (Dependency Inversion)  
  - spring solves web.xml nightmare, it introduced a new one - heavy XML or annotation-based configuration. setting up spring project requires hours of configuring database connections, security rules and dependency versions before you could write any actual business logic

- Spring boot
  - it uses opinionated defaults (auto-configuration) to automatically setup your application based on library you include. If you add database library, spring boot configures the connection automatically without you writing a single line of XML
  - comes with embedded server

- Core Features
  - Auto-configuration
    - @EnableAutoConfiguration
  - "Starter" dependencies
  - Embedded servers
  - Spring boot actuator
 
- Layered Architecture
  - Model-view-controller / MVC design pattern
  - request flow
    - client request hits the controller layer
    - service layer processes the logic
    - repository layer talks to the Database
    - Data flow back up as a Model

- Setting up spring project
	- spring initializer or start.spring.io
- Layered Architecture

![[Pasted image 20260721211339.png]]

- Maven
	-  It's project management tool, helps developers with
		- Build generation
		- Dependency resolution
		- Documentation etc
	- Maven uses POM - Project Object Model
	- When maven command is given, it looks for pom.xml in the current directory & get needed configuration
	- xml config
	- if you want to run specific goal of a particular phase all its previous phases needs to be finished
	- phases
		- validate - project structure
		- compile - source code
		- test - unit test
		- package - compiled code (war or jar)
		- verify the integrity of the pkg
		- install the package in local repository
		- deploy the package in remote repository
	- \<build> tag can be used to add new set of goals and task
	- validate - `mvn validate`
	- compile - `mvn compile`
		- it will validate and compile your code and put it together
	- test - `mvn test`
		- It will validate, compile and then run the TEST cases in your project
	- package - `mvn package`
		- first complete validate, compile, test phase and then run package phase in which it generates .jar or .war file
	- verify - `mvn verify`
		- it can perform some additional checks apart from unit test cases like
			- static code analysis
			- checksum verification etc
	- install - `mvn install`
		- It will install the .jar pkg in local maven repository
		- which is located in your user home directory ~/.m2/repository
	- deploy - `mvn deploy`
		- it will deploy the .jar to REMOTE repository

- Bean and its lifecycle
	- Simple term, bean is a Java Object, which is managed by Spring Container (also known as IoC container)
	- IoC container - contains all the beans which get created and also managed them
	- How to create a Bean ?
		- @Component Annotation
		- @Bean Annotation
	- @Component Annotation
		- @Component annotation follows "convention over configuration" approach
		- Means spring boot will try to auto configure based on conventions reducing the need for explicit configuration
		- @Controller, @Service etc. all are internally tells spring to create bean and manage it
	- @Bean comes into picture where we provide the configuration details and tells spring boot to use it while creating a Bean
	- Using @ComponentScan annotation, it will scan the specified package and sub-package for classes annotated with @Component, @Service etc
	- Through explicit defining of bean via @Bean annotation in @Configuration class

![[Pasted image 20260722100107.png]]
![[Pasted image 20260722100125.png]]

- Life cycle of bean
	- During application startup, spring boot invokes IOC container (ApplicationContext)
	- IOC Container, make use of Configuration and @CompoentScan to look out for classes for which beans needs to be created
	- Constructs the beans
	- Inject the Dependency into the Constructed Bean
	- @Autowired, first look for a bean of the required type
	- If bean found, spring will inject it, Different ways of injection
		- Constructor injection
		- Setter Injection
		- Field Injection
	- If bean is not found, spring will create one and then inject it
	- Perform any task before bean to be used in application
	- Use the bean in your application
	- Perform any task before bean is getting destroyed

- Annotations - Controller Layer
	- @Controller
		- It indicates that the class is responsible for handling incoming HTTP requests
	- @RestController
		- RestController = Controller + ResponseBody
	- @ResponseBody
		- Denotes that return value of the controller method should be serialized to HTTP response body
		- If we do not provide ResponseBody, spring will consider response as name for the view and tries to resolve and render it (in case we are using the @Controller annotation)
	- @RequestMapping
		- Value, path (both are same)
		- Method
		- Consumes, Produces
		- @Mapping
		- @Reflective ({ControllerMappingReflectiveProcessor.class})
	- @RequestParam
		- Used to bind, request parameter to controller method parameter
		- What is property Editor ?
	- @PathVariable
		- Used to extract values from the path of the URL and help to bind it to controller method parameter
	- @RequestBody
		- Bind the body of HTTP Request (typically JSON) to controller method parameter (java object)
	- ResponseEntity
		- It represents the entire HTTP response
		- Header, Status, response body etc

- Dependency Injection
	- Using dependency injection we can make our class independent of its dependencies
	- It helps to remove the dependency on concrete implementation and inject the dependencies from external source
- Issues without DI
	- classes becomes tightly coupled
	- It breaks DI rule of S.O.L.I.D principle
		- This principle says that do not depend on concrete implementation, rather depends on abstraction
- Different ways of injection
	- Field Injection
		- Dependency is set into the fields of the class directly
		- Spring uses reflection, it iterates over the fields and resolve the dependency
		- Adv
			- very simple and easy to use
		- Dis. Adv
			- can not be used with immutable fields
			- changes of null pointer exception
			- During unit testing setting MOCK dependency to this field becomes difficult
	- Setter Injection
		- Dependency is set into field using the setter method
		- we have to annotate the method using @Autowired 
		- Adv
			- Dependency can be changed any time after the object creation (as object can not be marked as final)
			- Ease of testing, as we can pass mock object in the dependency easily
		- Dis. Adv
			- Field can not be marked as final
			- Difficult to read and maintain
	- Constructor Injection
		- Dependency get resolved at the time of Object initialization itself
		- Its recommended to use
- Common issues
	- Circular dependency
		- @Lazy on field injection
	- Unsatisfied dependency
		- @Qualifier annotation

- Bean Scopes
	- Singleton
		- Default scope
		- Only 1 instance created per IoC
		- Eagerly initialized by IoC, means at the time of application startup, object get created
	- Prototype
		- Each time new object is created
		- its Lazy initialized, means when object is created only when its required
		- @Scope("prototype") 
	- Request
		- New object is created for each HTTP request
		- Lazily initialized
		- @Scope("request")
	- Session
		- New Object is created for each HTTP session
		- Lazily initialized
		- When user accesses any endpoint, session is created
		- Remains active, till it does not expires

- Dynamic bean initialization
	- **UnsatisfiedDependencyException** occurs when there is multiple implementation of the Interface exist and with @Autowired system does not know which one to inject
	- @Qualifier can be used to define the implementation which should be injected
	- @Value it is used to inject value from various sources like property file, environment variables or inline literals

- @ConditionalOnProperty
	- Bean is created conditionally (mean bean can be created or not)
	- What if we have below use cases
		- We want to create only 1 bean, either MySQLConnection or NoSQLConnection
		- We have 2 components, sharing same codebase, but 1 component need MySQLConnection and other needs NoSQLConnection
	- Adv
		- Toggling of feature
		- Avoid cluttering Application context with un-necessary beans
		- Save memory
		- Reduce application Startup time
	- Dis. Adv
		- Misconfiguration can happen
		- Code complexity when over used
		- Multiple bean creation with same Configuration, brings confusion
		- Complexity in managing

- @Profile
	- we put the configuration in "application.properties" file but how to handle, different environment configurations ?
	- that's where profiling comes into the picture
		- application.properties
			- application-\<prof1>.profile
			- application-\<prof2>.profile
			- application-\<prof3>.profile
			- ...
			- application-\<profN>.profile
	- During application startup, we can tell spring boot to pick specific "application.properties" file, using "spring.profiles.active" configuration
	- We can pass the value of this configuration "spring.profiles.active" during application startup itself
	```
	mvn spring-boot:run -Dspring-boot.run.profiles=prod
	```
	- using @Profile annotation, we can tell spring boot, to create bean only when particular profile is set

