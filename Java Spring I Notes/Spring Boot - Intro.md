
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

