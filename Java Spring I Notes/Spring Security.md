
Common Attacks

- CSRF
	- Cross site request forgery
	- An attacker tricks a logged-in user's browser into executing an unwanted action on a trusted site. For example, if you are logged into your bank, a malicious site you visit could silently send a request to your bank to transfer money. The bank sees the request coming from your browser (with your valid session cookie) and processes it.
	- protection in spring boot
		- default protection - enables all REST APIs which can change the state
		- It uses a synchronized token pattern. The server generates a unique, unpredictable CSRF token for the session. Any state-changing request must include this exact token (usually as a hidden form field or an HTTP header). If the token is missing or incorrect, Spring rejects the request
	- Never disable CSRF protection (e.g., `http.csrf(csrf -> csrf.disable())`) in production unless you are building a stateless REST API (like one using JWTs) where session cookies are not used for authentication.
- XSS
	- Cross site scripting
	- An attacker injects malicious client-side scripts (usually JavaScript) into web pages viewed by other users. If a site displays a user's comment exactly as typed (e.g., `<script>stealCookies();</script>`), every user viewing that comment will execute the script.
	- protection in spring boot
		- f you use a templating engine like Thymeleaf, tags like `th:text` automatically escape HTML characters by default, rendering the script harmless text rather than executable code.
		- Spring Security automatically adds the `X-XSS-Protection` header to HTTP responses. While modern browsers rely more on Content Security Policy (CSP), Spring makes it easy to configure CSP headers to restrict where scripts can be loaded from and executed (e.g., blocking inline scripts)
		- While output encoding is preferred, you can also use libraries like Jsoup to strip malicious HTML tags before saving data to your database.
- CORS
	- Cross-Origin Resource-Sharing
	- CORS is actually a _security feature_ of modern browsers, not an attack itself. Browsers enforce the "Same-Origin Policy," meaning a web page served from `domain-a.com` cannot normally make AJAX requests to `domain-b.com`. CORS is the protocol that allows the server (`domain-b.com`) to explicitly tell the browser, "Yes, it is safe to let `domain-a.com` access my resources."
	- protection in spring boot
		- By default, browsers block cross-origin requests, and Spring Boot will not process them unless explicitly configured.
		- ou must configure Spring to send the correct CORS headers (like `Access-Control-Allow-Origin`). This can be done globally in your Security Filter Chain by configuring a `CorsConfigurationSource` bean, or locally on specific controllers using the `@CrossOrigin` annotation.
	- Avoid using `setAllowedOrigins(Arrays.asList("*"))` (allowing all origins) in production, as this defeats the purpose of the Same-Origin Policy. Explicitly list the trusted domains (e.g., your frontend application's URL).
- SQL Injection
	- An attacker inputs malicious SQL statements into an application's input fields. If the application insecurely concatenates this input directly into a database query, the attacker's SQL will be executed, potentially allowing them to read, modify, or delete the entire database.
	- protection in spring boot
		- Parameterized queries / prepared statements
		- Spring Data JPA

- To prevent from these types of various attacks we need to ensure
	- Authentication - Verify who you are
	- Authorization - Check what you are allowed to do
- This is where spring security comes into picture
- Dependency `spring-boot-starter-security`

- Security filter chain in main component in implementing authentication and authorization

![[Pasted image 20260729092307.png]]

- Few Authentication and Authorization mechanism
	- Form Login (Stateful)
	- Basic Authentication (Stateless)
	- JWT (Stateless)
	- OAuth2
		- Authorization Code (Stateful or stateless)
		- Client credentials (Stateless)
		- Password Grant (Stateless)
	- API Key Authentication (Stateless)

- User creation
	- InMemoryUserDetailsManager
		- default user with username user
		- password is random string
		- resets on every server restart
	- SecurityProperties.java file contains default values for security configuration
	- We can manager by creating custom InMemoryUserDetailsManager Bean
		- The default format for storaing the password is : {id}password
		- {id} can be either
			- {noop}
			- {bcrypt}
			- {sha256}
			- etc
- PasswordEncoder
	- DelegatingPasswordEncoder
	- BCryptPasswordEncoder
	- NoOpPasswordEncoder
- UserDetailsService
- PasswordEncoder @Bean registration to define single format of the password encoding
- Storing UserName and Password (after hashed) in DB - recommended for production
	- UserAuthEntity should implement UserDetails
		- Few methods to override like getAuthorities(), isAccountNonExpired(), isAccountNonLocked() etc
	- UserAuthEntityRepository
		- findByUsername()
	- UserAuthEntityService
		- loadUserByUsername()
		- save()
	- UserAuthController
		- /register endpoint controller handler
- By default all endpoints in spring boot is AUTHENTICATED so to register our self /auth/register API needs to be relax
	- By overriding @Bean for securityFilterChain(HttpSecurity http) with authorizeHttpRequests, csrf etc config and builder chain method

