
LLD Foundations

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

UML & Its Applications
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

Clean Code

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