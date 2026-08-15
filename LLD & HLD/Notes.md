
### Q. How to approach LLD OOD Interview problem

- Tests ability to translate high level requirements into detailed class structures, methods and their interactions using object-oriented design principles
- Steps
	- Clarify requirements
	- Identify entities
	- class design
	- implementation
	- exception handling
- Clarify requirements / use cases
	- core features to be supported
	- any specific feature to be prioritize
	- primary users of the system
	- actions that user can take
	- any specific constraints or limitations
	- do we need to handle concurrency
	- errors, edge cases, exceptions and unexpected inputs
- Identify Entities / problem analysis
	- After clear with requirements, break down the problem and identify the core entities or objects we need to have in the design
	- core entities are the key objects around which our system is built
	- these entities will become the class in object oriented design like a nouns in the problem description
- Class Design
	- Next step is to design the `classes`, `enums` and `interfaces`
	- Define classes and relationships
		- Translate entities into classes and come up with a list of attributes you want to have in those classes
		- If design have multiple classes, figure out how would they would relate with each other.
		- Optional `UML class diagram` creation
		- One to Many, Many to Many, Many to One relation design between classes with HashMap & ArrayList
	- Define interfaces and core methods
		- Define interfaces and core methods for each classes
		- Methods encapsulate the actions or behaviors that each class is responsible for, they are the verbs associated with entities
		- define method signatures with its parameters and its purpose
	- Define a central class
		- We don’t want to manipulate classes in our design directly from outside, that’s why we need a central class that provides a unified interface for interacting with the system.
		- That will serve as the central coordinator for the entire system.
		- It manages the creation, retrieval, and interaction of all major components.
- Implementation / Code Quality
	- Follow good coding practices
		- Use meaningful names for classes, methods and variables
		- Focus on simplicity and readability
		- Favor composition over inheritance to promote flexibility and avoid tight coupling
		- avoid duplicating code or logic
		- Use interfaces to define contracts and enable loose coupling between components.
		- Only implement what is required.
		- Strive for modularity and separation of concerns to make the codebase maintainable and scalable.
		- Apply design principles and design patterns wherever necessary.
		- Make your code scalable so that it performs well with large data sets.
	- Implement necessary methods
		- Check with the interviewer to understand which methods are important for the interview.
	- Address Concurrency
		- Handle race conditions
		- check where we need to handle concurrency in design
		- few strategies to address concurrency
			- use synchronization
			- use atomic operations
			- use immutable objects
			- use thread safe data structures
- Exception handling
	- The problem may require you to handle **errors**, **edge cases**, **exceptions**, and **unexpected input**.
- Extensibility & maintainability
	- Think about 3 or 4 / be ready with at least 2 follow ups and expected to propose a change without major refactoring

### Design Principles

- General software design principles
	- KISS
	- DRY
	- YAGNI
- KISS 
	- Keep it simple, stupid
	- The simplest solution that works is usually the right one. When you're designing a class or choosing between patterns, pick the straightforward approach.
	- The time to add complexity is when simplicity stops working. If your single class grows to 500 lines with ten different responsibilities, that's when you refactor.
	- start simple
- DRY
	- Don't repeat yourself
	- When you find yourself writing the same logic in multiple places, pull it into one place. If three classes all validate email addresses the same way, create a shared validation method. If two services both need to convert timestamps, put that conversion in a utility function.
	- DRY also conflicts with KISS. Sometimes the simplest solution is to duplicate code in two places rather than build an abstraction. There's no right answer, and showing you understand this tradeoff is what separates senior candidates.
- YAGNI
	- You Aren't Gonna Need It
	- Build what you need now, not what you might need later.
	- Don't make your classes extensible in every direction just in case.
	- The problem with building for future requirements is you usually guess wrong. You add complexity for scenarios that never happen, and when the actual new requirement comes, it's different from what you prepared for. Now you're stuck maintaining dead code.
	- **This principle doesn't mean "never think ahead" - it means don't build ahead. Design with extension in mind, but only implement what's needed now.**
	- Your initial design, stick to what's actually needed.
- Separation of Concerns
	- Different parts of your code should handle different responsibilities, and they shouldn't know about each other's internals.
	- Your UI layer shouldn't contain business logic. Your business logic shouldn't know how data is stored. Your data access layer shouldn't format strings for display.
- Law of Demeter
	- Known as principle of least knowledge
	- A method should only talk to its immediate friends, not reach through objects to access distant parts of the system.
	- The problem with deep chaining is coupling. Your code now knows the internal structure of three different objects.
	- In interviews, this comes up when you're defining class methods. Instead of returning complex objects that callers need to dig through, return the specific data they need or provide higher-level methods that do the work.

- Object Oriented Design Principles (SOLID)
	- SRP
		- Single Responsibility principle
		- A class should have one reason to change. If a class mixes multiple concerns, split them. This is the foundation of good class design.
	- OCP
		- Open/Closed Principle
		- Classes should be open for extension but closed for modification. You should be able to add new behavior without changing existing code. This usually means using interfaces or abstract classes so you can add new implementations without touching the original code.
		- Every time you modify existing code, you risk breaking things that already work. If you design with interfaces from the start, adding new functionality becomes a matter of writing new classes that implement those interfaces. The old code never changes, so it can't break.
	- LSP
		- Liskov Substitution Principle
		- Subclasses must work wherever the base class works. If you have a method that accepts a Bird, passing in a Penguin shouldn't break things even though penguins can't fly. This means your subclasses can't violate the expectations set by the parent class.
		- This comes up in interviews when you're designing class hierarchies. Think carefully about what methods belong in the base class versus subclasses.
	- ISP
		- Interface Segregation Principle
		- Prefer small, focused interfaces over large, general-purpose ones. Don't force classes to implement methods they don't need. If a class only needs two methods from an interface with ten methods, that interface is too big.
		- The problem with fat interfaces is that classes are forced to implement methods they'll never use. This leads to empty implementations or methods that throw exceptions, which is a code smell. Split large interfaces into smaller, cohesive ones. Classes can implement multiple small interfaces if they need to, but they're not stuck implementing irrelevant methods.
	- DIP
		- Dependency Inversion Principle
		- Dependency Inversion states that your code should depend on abstractions, not concrete implementations. Instead of `NotificationService` creating an `EmailSender` directly, it should accept a `MessageSender` interface through its constructor.
		- The "inversion" refers to who defines the contract. Normally, your business logic conforms to whatever the implementation provides. With DIP, you flip this: define an interface based on what your business logic needs, then have implementations conform to that interface. The implementation adapts to the business logic, not the other way around.





