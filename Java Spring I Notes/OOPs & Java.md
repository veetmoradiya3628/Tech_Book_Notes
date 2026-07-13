
- OOPs
	- Procedural vs. Object Oriented Design
	- Object has 2 things
		- Properties or state
		- Behavior or Function
	- Class is a blueprint / skeleton of an object
	- Pillar of OOPs
		1. Data Abstraction
			- It hides the internal implementation and shows only essential functionality to the user
			- Ex. Car we only show the brake pedal and if we press it car speed will reduce. but how ? that is abstracted away from user.
		2. Data Encapsulation
			- Encapsulate bundles the data & code working on that data in a single unit.
			- also known as data hiding
			- getter and setter functions
		3. Inheritance
			- Capability of a class to inherit properties from their parent class
			- Achieved using extends keyword or through inheritance
			- Types of inheritance
				- Single inheritance
				- Multilevel inheritance
				- Hierarchical inheritance
				- Multiple inheritance (Not actually supported by Java due to diamond problem but through interfaces we can solve the diamond problem)
		4. Polymorphism
			- Poly means "Many" & morphism means "Form"
			- A same method behaves differently in different situation
			- Types of polymorphism
				- Compile time / Static polymorphism / Method overloading
				- Run time / Dynamic polymorphism / Method overriding
					- Everything as arguments, return type, method name is same
	- IS-A relationship
		- Achieved through inheritance
		- Ex. Dog IS-A Animal
	- HAS-A relationship
		- Whenever an object is used in other class, it's called HAS-A relationship
		- Relationship could be one-to-one, one-to-many, many-to-many etc
		- Ex
			- school has students
			- bike has engine
			- school has classes
		- Association - relationship between 2 different objects
			- Aggregation - Both objects can survive individually, means ending of one object will not end another object
			- Composition - Ending of one object will end another object

- Java is
	- Platform Independent Language
	- Supports OOPs
	- Portability - WORA (Write once run anywhere)
- 3 Main components of Java
	- JVM
	- JRE
	- JDK
	
![[Pasted image 20260713085657.png]]

- JVM - Java Virtual Machine
	- It's just an abstract machine that does not exist physically
	- JVM is platform dependent
	![[Pasted image 20260713085837.png]]
	- Input of JVM is bytecode & output is machine code
	- since bytecode can be run by any JVM, it makes a Java program platform independent
	- JVM has JIT (Just-In-Time) compiler which takes bytecode & convert it into machine code
- JRE - Java Runtime Environment
	- JRE contains JVM & class libraries (the libraries which we have used in our code)
	- JRE = JVM + Class libraries
	- if we have JRE we can run any Java program but we can not code the program
- JDK - Java Development Kit
	- It has programs language information
	- It has compiler (javac)
	- It has debugger
	- so JDK = JRE + (program language + compiler + debugger + other dev. components)
- So JVM, JRE and JDK are platform dependent but the compiled bytecode is platform independent

- JSE - Java Standard edition
	- its core java
- JEE - Java Enterprise edition | Jakarta (EE)
	- JSE + Servlets + JSP + Transaction API + Persistent API
- JME - Java Micro / Mobile edition
	- API for mobile applications

- Java Variables
	- Primitive Data Types 
		- Variable
		- Datatype variableName = value;
	- Java is static types language i.e. we mandatorily have to define the datatype of a variable
	- Java is strongly typed language i.e. there is restriction on what value can be assigned to the variable
	- Variable naming convention
- Types of variables
	- Primitive type
		- char
		- byte
		- short
		- int
		- long
		- float
		- double 
		- boolean
	- Non-primitive type / Reference type
		- class
		- interface
		- array
		- string
		- enum
- char
	- 2 bytes (16 bits)
	- Character representation of ASCII values 
	- Range: 0 to 65535 
	- Default value is null
- byte
	- 1 byte (8 bits)
	- -128 to 127
	- Signed 2s complement
	- default value is 0
	- 2s complement = complement  + 1
- short
	- 2 bytes (16 bits)
	- signed 2s complement
	- -32768 to 32767
	- Default value is 0
- int
	- 4 bytes (32 bits)
	- -2^31 to 2^31 - 1
	- Default value is 0
	- Signed 2s complement
- long
	- 8 bytes (64 bits)
	- singed 2s complement
	- -2^63 to 2 ^ 63 - 1
	- Default value is 0
- boolean
	- 1 bit
	- true or false
	- default value is true

- Types of conversion
	1. Widening / Automatic conversion
		- Automatic conversion when we go from lower data type to higher data type
		- byte to short to int to long
	2. Narrowing / Down casting / Explicit conversion
		- It is opposite of widening i.e. going from higher data type to lower datatype
		- In this case down casting doesn't happen automatically, so we have to manually do it
	3. Promotion during expression
		- This happens internally during expression
		- As soon as value of expression crosses the range of the datatype then promotion happens internally to higher datatype
		- byte & short promotes to int
		- In expression if any one datatype is higher then all other data types will also be converted to higher data type

- kind of variables
	- Member / Instance variable
		- Defined inside class object
	- Local variable
		- Defined inside method
	- Static / class variable
		- Only one copy of static / class variable exists. all objects can access it using class name
	- Method parameters
		- These are the variables that are passed to a method
	- Constructor parameters
		- There are the variables that are passed to a constructor

- Fractional types
	- how float / double are stored in memory ?
		- 1 bit + 8 bits + 23 bits
			- 1 bit - stores sign (0 - positive, 1 - negative)
			- 8 bits - stores exponent
			- 23 bits - stores significant
	- how does binary conversion of fractional point number works ?

- Reference data types / Non-primitive data types
- 4 data types
	- Class
	- String
	- Interface
	- Array
- In java, everything is pass by value. so with the help of reference variables we're achieving the functionality of pointers in cpp

- Strings
	- Strings are immutable in Java
	- It contains string literal
	- Inside heap, there is a fixed memory space k/a string constant pool so the string variable holds a reference of corresponding string literal in string constant pool
- Interface
	- we can store the objects of a child in a parent one or we can store the objects in the same class itself but we cannot create an object of interface
- Array
	- Sequence of memory storing the same datatype
	- 1D, 2D, 3D,... ND
- Wrapper classes
	- Autoboxing
		- To convert a primitive data type to its wrapper
	- Unboxing
		- To convert a wrapper class to primitive
	- For each of the primitive data types we have corresponding reference types that are known as wrapper classes
	- It gives advantages over a primitive types
	- The collections works on objects only. i.e on reference data types so we need wrapper class to use collections
- Constant variable
	- created using `final` keyword and we can not change its value once created
