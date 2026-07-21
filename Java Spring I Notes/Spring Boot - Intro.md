
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

