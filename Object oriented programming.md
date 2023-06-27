- method of structuring a program
	- involves bundling related properties and behaviors into individual objects
- objects are like the components of a system
- program = factory assembly line
- at each step of the assembly line, a system component processes some material
	- ultimately transforming raw material into a finished product
- an object contains
	- data
		- like the raw or preprocessed materials at each step
	- behavior
		- action each assembly line component performs

- object-oriented programming is a programming paradigm
	- programming paradigm: different ways or styles in which a given program or programming languages can be organized
		- some paradigms are mainly concerned with the execution model of the language
- class is a user-defined-data-type
	- consists data members and member functions
	- blueprint for an object
- object
	- instance of a class

#2023-06-25
- unified modeling language
	- standardized notation for diagrams to visualize object-oriented systems
		- notation: system of symbols that represent information
- another common programming paradigm is procedural programming
	- structures a program like a recipe
		- provides set of steps
			- in the form of functions and code blocks
		- flows sequentially
		- end goal is to get from point A to point B to complete some task
- in oop
	- each object is described and what each one can do
	- we do not provide sequence of steps
	- instead of writing a single large program, the oop is split into several self-contained objects
		- several mini-programs
			- where each objects contains its own data and logic to describe how it behaves and interacts with other objects
	- the end results of pp and oop are same
	- neither is better than the other
	- one main adv of oop: code re-usability
		- meaning further objects can be created with an existing object
	- the same objects we created might be useful for slightly diff cases as well
	- oop is very useful for projects of large scale
- objects
	- same definition as in the real world
		- ex: coffee mugs
			- two coffee mugs are not same object
			- similar mugs
	- every object has characteristics, inherent properties that describe its current state
		- a mug can be full, empty, or somewhere in b/w
		- filling one mug with coffee does not mean all of the mugs in the world get filled
	- it is possible for one object to contain other objects
	- identity, attributes and behaviors are three things that describe objects in an object-oriented programming language
	- every object in a computer program is self-contained, it has its own:
		- identity separate from other objects
		- attributes to describe its current state
		- behaviors 
	- nouns = objects
- classes
	- used to create objects
	- detailed description, definition, the template of what an object will be
	- not the object itself
		- classes can be thought of cookie cutters
	- each class has three components
		- class name
		- attributes to describe an object
		- behaviors
	- methods are just functions defined as part of a class
	- can only access data that is known to that object
	- process of creating an object: instantiation
		- because each object that is created is an instance of that particular class
![[Pasted image 20230625105637.png]]


#2023-06-26
## Abstraction
- focus on the essential qualities of something rather than one specific example
- idea or concept of a something is completely separate from any specific instance
- we don't create classes for each instance
	- we focus on the essential qualities of the idea that one cares about

## encapsulation
- about containing the elements of an object
	- not just to keep them together
	- but to also protect them
- we bundle an object's attribute or data along with the methods that operate on that data within the same unit or class
	- we do not want just anyone to do anything they want with the object's element
	- we introduce a systematic way of working with the elements
- one of the principles of encapsulation: an object should not make anything about itself available except what is absolutely necessary for other parts of the application to work
	- this is called black boxing
	- we are closing off inner workings of the objects, while only revealing some inputs and outputs
- one of the main benefits with object orientation is that it allows us to more safely change the way that the object works, without changing the rest of the application
- encapsulate as much as possible

## Inheritance
- base a new class or object on an existing one
- inherit the existing attributes and methods
![[Pasted image 20230626182606.png]]
- no limit to the number of derived class from a single base class
- code reuse
- propagate changes elegantly

## Polymorphism
- having many forms
- dynamic(or runtime) polymorphism: allows us to access methods using the same interface on different types of objects that may implement those methods in different ways
	- overriding 
	- defining diff classes that can be used with the same interface
- static(compile-time) polymorphism
	- implements multiple methods with the same name, but diff parameters
	- usually overloaded methods provide different but very similar functionality
	- same name diff sets of params