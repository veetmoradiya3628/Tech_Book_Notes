
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

