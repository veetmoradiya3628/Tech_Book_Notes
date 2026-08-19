
### LLD Foundations
- Encapsulation
	- Encapsulation is the practice of bundling data (state) and the methods that operate on that data into a single unit, while restricting direct access to some of the object's components. You expose only what is absolutely necessary.
	- **Analogy:** A coffee machine. You don't need to know how the internal heater or grinder works; you just press a button (the public interface) to get coffee.
	- Java classes
	- Go struct upper case and lower case parameter 

- Association
	- Knows-a
	- Association is a general relationship between two independent objects. They interact with each other, but neither "owns" the other, and they have separate lifecycles.
	- **Analogy:** A **Doctor** and a **Patient**. A doctor sees many patients, and a patient can see many doctors. If the doctor retires, the patient still exists.
```
public class Doctor {
    private String name;
    // Doctor knows about the Patient
    public void examine(Patient patient) { 
        System.out.println("Examining " + patient.getName());
    }
}
```

- Aggregation
	- Has-A
	- Aggregation is a specialized form of association. It represents a "whole-part" relationship, but the "part" can exist completely independent of the "whole".
	- **Analogy:** A **Department** and a **Professor**. A department has professors. If the physics department is shut down, the professors still exist and can move to another university.
```
public class Department {
    private List<Professor> professors;

    // Professors are created outside and passed in
    public Department(List<Professor> professors) {
        this.professors = professors;
    }
}
```

- Composition
	- Part-of
	- Strong containment
	- Composition is a strict "whole-part" relationship. The "part" **cannot** exist without the "whole". If the parent is destroyed, the child is destroyed.
	- **Analogy:** A **House** and a **Room**. If you demolish the house, the rooms no longer exist.
```
public class House {
    private Room livingRoom;

    public House() {
        // House strictly creates and owns the Room
        this.livingRoom = new Room("Living Room"); 
    }
}
```

- Generalization
	- IS-A, inheritance
	- Generalization is the process of extracting shared characteristics from two or more classes and combining them into a generalized superclass.
	- **Analogy:** A **Dog** "is an" **Animal**.
```
public class Animal {
    public void eat() { System.out.println("Eating..."); }
}

public class Dog extends Animal {
    public void bark() { System.out.println("Woof!"); }
}
```

- Dependency
	- Uses-a
	- Dependency is a weak, often temporary relationship where a change in one class forces a change in another. It usually occurs when a class uses another class as a method parameter, local variable, or return type.
	- **Analogy:** A **Report** uses a **Printer**. The report doesn't own the printer, it just needs it momentarily to print itself.
```
public class Printer {
    public void printDocument(String text) { /* ... */ }
}

public class Report {
    // Dependency: Report depends on Printer temporarily
    public void generate(Printer printer) {
        printer.printDocument("Annual Data");
    }
}
```

- Polymorphism
	- Polymorphism allows different classes to be treated as instances of the same class through a common interface.
	- **Analogy:** A **USB Port**. The port doesn't care if you plug in a mouse, a keyboard, or a webcam, as long as it implements the "USB Interface".
```
public interface Notification {
    void send(String message);
}

public class EmailNotification implements Notification {
    @Override
    public void send(String message) { System.out.println("Emailing: " + message); }
}
```

- Abstraction
- Association vs. Dependency
	- **Association** is a structural relationship. It is a long term connection where one object remembers another as part of its state.
	- **Dependency** is a _behavioral_ relationship. It is a short-term, temporary connection where one object just needs another to complete a specific task.
- High Cohesion and Loose (Low) Coupling

- Cohesion
	- **Cohesion** measures how closely related the responsibilities of a single class, struct, or module are.
	- High cohesion is good
		- A module does _one_ thing and does it well. Its methods and properties are highly related. (This aligns directly with the Single Responsibility Principle).
	- Low cohesion is bad
		- A module is a "God object" or a dumping ground for unrelated tasks (e.g., handling databases, formatting text, and sending emails all in one place).

- Coupling
	- **Coupling** measures how much two separate modules depend on each other.
	- Loose coupling is good
		- Modules know very little about each other. They interact through well-defined contracts (interfaces). If you change module A, module B doesn't break.
	- Tighe coupling is bad
		- Modules are deeply intertwined, often relying on concrete implementations. A change in one module forces a cascading change in several others.



- filled diamond denotes composition, meaning strong ownership. engine’s lifecycle depends on Car.
 - dashed arrow indicates dependency, meaning AuthService uses Database temporarily.
 - dashed arrows in sequence diagrams represent return values or responses, not calls.
 - multiplicity 0..* means many.  so one Teacher can be associated with multiple Students.
 - A self-call (arrow starting and ending on the same lifeline)

### UML & Its Applications
- Class Diagrams - structural
	- Class diagrams show the static structure of your system: what entities exist, what data they hold, and exactly how they relate to one another.

![[UML Class Diagram.png]]

- Inheritance / Generalization - IS-A - `extends`
- Realization - Implements - `implements`
- Composition / Strong Has-A
- Aggregation / Weak Has-A
- Dependency / USES-A

- Sequence diagram - Behavioral 
	- shows how objects communicate over time to fulfill a specific use case.
	- Key components
		- Lifeline
		- Activation bar
		- Synchronous message
		- Return message
- State Machine diagram / State diagram
	- A State Diagram models the lifecycle of a _single_ object. It shows all the different statuses (states) that object can be in, and the specific events (triggers) that cause it to move from one state to another.
	- Components
		- State
		- Transition
		- Trigger / Event

### Clean Code

- Separation of concerns
- Small focused methods improve readability and make validation logic easier to maintain and test.
- Guard clause - bouncer pattern
	- Avoid deep nesting and the "arrow anti-pattern" (where code keeps indenting to the right). Instead of wrapping your main logic in a giant `if (isValid)` block, handle the negative cases and throw exceptions or return at the very top of the method.
- Tell, don't ask (encapsulation of behavior)
- The Law of Demeter (Avoid "Train Wrecks")
- Principle of Least Astonishment (POLA)
	- Your methods should do exactly what the name suggests and nothing more. Side effects are the enemy.
- Command-Query Separation (CQS)
	- A method should either be a _Command_ (changes state, returns void) or a _Query_ (returns data, doesn't change state). Don't mix them. A method named `validateBooking()` should not also write the booking to the database.
- **Meaningful Signatures:** Method names should read like English. `process()` tells you nothing. `calculateFinalPriceAfterDiscount()` is self-documenting.
- **Minimize Scope:** Keep variables as tightly scoped as possible. Decompose large methods into smaller, private helper methods that do exactly one thing.
- Method extraction breaks large logic into smaller readable units with focused responsibilities.
- Direct dependency on concrete implementations makes systems harder to extend and modify.
- Poor cohesion occurs when unrelated responsibilities exist inside the same class.

**1. Report Generation System Refactor**
- **Short Description**
	- A report generation system refactored from a God Class into dedicated data provider, formatter, and renderer components, allowing the service to coordinate the workflow while keeping data processing, formatting, and rendering responsibilities separate.
- **Topics Used**
	- God Class, Separation of Concerns, Cohesion, Responsibility Separation, Refactoring
- **Concepts Practiced**
    - Identifying God Class code smell, extracting data management responsibilities, formatter separation, renderer extraction, workflow orchestration, cohesive class design, helper class collaboration, maintainable service architecture.

**2. Storage Management System**
- **Short Description**
    - A storage management system refactored to replace direct dependency on a concrete storage implementation with a common abstraction, enabling the service to work with multiple storage types without modification.
- **Topics Used**
    - Tight Coupling, Dependency Inversion, Programming to Abstractions, Interface-Based Design, Refactoring
- **Concepts Practiced**
    - Removing tight coupling, introducing abstractions, interface implementation, dependency injection through constructors, interchangeable storage implementations, extensible service design, delegation, maintainable architecture.

**3. Student Result Processing System**
- **Short Description**
    - A student result processing system refactored from a long workflow method into smaller focused helper methods that calculate marks, evaluate results, generate summaries, and coordinate the overall processing flow.
- **Topics Used**
    - Long Method, Mixed Responsibility, Method Extraction, Refactoring
- **Concepts Practiced**
    - Workflow decomposition, total marks calculation, average computation, pass/fail evaluation, summary generation, helper method extraction, readable execution flow, maintainable business logic organization.

**4. Notification Delivery System**
- **Short Description**
    - A notification delivery system designed around a common notification abstraction, allowing multiple notification channels to provide their own message delivery behavior while enabling uniform processing through polymorphism.
- **Topics Used**
    - Polymorphism, Interfaces, Runtime Polymorphism, Loose Coupling, Class Diagram
- **Concepts Practiced**
    - Designing interface-based systems, implementing multiple concrete classes, runtime method dispatch, programming to abstractions, interchangeable notification implementations, collection of interface references, extensible notification architecture.

**5. Inventory Restock Processing System**
- **Short Description**
       - An inventory management system refactored from a large workflow method into smaller focused methods that handle validation, stock updates, cost calculation, inventory evaluation, and summary generation.
- **Topics Used**
    - Long Method, Mixed Responsibility, Method Extraction, Refactoring
- **Concepts Practiced**
    - Workflow decomposition, validation extraction, inventory status evaluation, stock update processing, helper method design, readable execution flow, responsibility separation within service methods, maintainable business logic organization.

**6. Customer Loyalty Evaluation System**
- **Short Description**
    - A customer loyalty evaluation system refactored from a monolithic processing method into smaller cohesive methods that calculate loyalty scores, determine membership categories, and evaluate reward eligibility.
- **Topics Used**
    - Long Method, Mixed Responsibility, Method Extraction, Refactoring
- **Concepts Practiced**
    - Business rule extraction, loyalty score calculation, membership classification logic, reward eligibility evaluation, workflow coordination, helper method organization, clean processing flow, maintainable service design.

**7. Resume Builder System**
- **Short Description**
    - A resume generation system refactored from a God Class into specialized formatter and validation components to improve cohesion, readability, and responsibility separation.
- **Topics Used**
    - God Class, Separation of Concerns, Cohesion, Responsibility Separation, Refactoring
- **Concepts Practiced**
    - Identifying God Class code smell, extracting formatting responsibilities, validator separation, cohesive class design, formatter-based architecture, resume generation workflow orchestration, maintainable class structure, improved readability through focused components.

### SOLID

- S - Single Responsibility Principle
- O - Open Closed Principle
	- New behavior was added without modifying existing workflow logic.
- L - Liskov Substitution Principle
	- Different implementations safely replace abstraction references without breaking workflow behavior.
- I - Interface Segregation Principle
- D - Dependency Inversion Principle
	- It means that big parts of your program should not connect directly to small, detailed parts. Instead, both should rely on simple general rules or interfaces
- Down casting dependency on concrete implementation breaks abstraction-driven design and increases coupling to implementation details.
- Polymorphism using abstraction
- It means that big parts of your program should not connect directly to small, detailed parts. Instead, both should rely on simple general rules or interfaces

Ex. 
**1. Multi-Channel Alert System**
- **Short Description**
    - A notification platform that supports multiple alert channels while separating notification delivery from history tracking and allowing new notification types to be added without modifying existing workflow logic.
- **Topics Used**
    - SRP, OCP, Abstraction, Polymorphism
- **Concepts Practiced**
    - Responsibility separation between sending and tracking, interface-driven design, extensible notification channels, workflow coordination through services, history management, interchangeable notification implementations, open-for-extension architecture.

**2. Logistics Fleet Management System**
- **Short Description**
    - A delivery management system that supports multiple delivery partner types through interchangeable implementations while keeping delivery workflows independent of partner-specific logic.
- **Topics Used**
    - OCP, LSP, Polymorphism, Substitutability
- **Concepts Practiced**
    - Interface-based delivery abstraction, interchangeable partner implementations, safe substitutability, workflow delegation, extensible delivery architecture, behavior replacement without workflow modification, abstraction-driven design.

**3. Online Shopping Workflow System**
- **Short Description**
    - An e-commerce order processing platform that coordinates payment processing, notifications, delivery assignment, and invoice generation through extensible and loosely coupled components.
- **Topics Used**
    - SRP, OCP, LSP, Abstraction, Polymorphism
- **Concepts Practiced**
    - End-to-end workflow orchestration, responsibility separation across components, interchangeable payment mechanisms, interchangeable notification channels, interchangeable delivery partners, abstraction-driven architecture, safe substitutability, extensible business workflow design.

**4. Employee Payroll Management System**
- **Short Description**
    - A payroll processing platform that separates salary calculation, tax computation, payslip generation, and workflow coordination into dedicated components connected through abstractions.
- **Topics Used**
    - SRP, DIP, Responsibility Separation, Loose Coupling
- **Concepts Practiced**
    - Dependency inversion through calculation abstractions, payroll workflow orchestration, salary and tax processing separation, payslip generation, reusable business components, loosely coupled service design, extensible payroll architecture.

**5. Smart Restaurant Management System**
- **Short Description**
    - A restaurant workflow management system that assigns responsibilities through small role-based interfaces so employees depend only on behaviors they actually perform.
- **Topics Used**
    - ISP, SRP, Clean Interface Design, Responsibility Separation
- **Concepts Practiced**
    - Interface segregation, role-based abstractions, focused employee responsibilities, avoiding fat interfaces, workflow coordination through interfaces, clean dependency management, behavior-driven design.

**6. Notification Delivery Platform**
- **Short Description**
    - A notification platform that supports multiple delivery providers while keeping notification delivery, history tracking, and workflow coordination loosely coupled and extensible.
- **Topics Used**
    - SRP, OCP, DIP, Abstraction, Extensible Design
- **Concepts Practiced**
    - Dependency inversion through notification abstractions, separation of delivery and tracking responsibilities, extensible provider architecture, workflow delegation, interchangeable notification implementations, history management, loosely coupled system design.

**7. Ride Dispatch & Transport Allocation System**
- **Short Description**
    - A transport dispatch platform that supports interchangeable transport partners while separating route planning from dispatch execution through clean responsibility boundaries.
- **Topics Used**
    - LSP, SRP, Polymorphism, Substitutability, Responsibility Separation
- **Concepts Practiced**
    - Transport partner substitutability, route planning separation, dispatch workflow orchestration, polymorphic transport execution, abstraction-driven design, extensible transport architecture, clean responsibility allocation.

