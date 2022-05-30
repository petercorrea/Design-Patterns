---
attachments: [factory.png]
tags: [Design Patterns]
title: Behavioral Patterns - Visitor
created: "2021-05-08T17:52:15.782Z"
modified: "2021-05-08T18:14:31.771Z"
---

// VISITOR
// Intent:
// seperate algorithms from the objects they operate on
// Design:
// text

# Behavioral Patterns - Visitor

###### Design patterns enable us to refactor code to provide clarity and intent through structure. They allow us to apply general solutions to common problems. Behavioral patterns are concerned with algorithms and the assignment of responsibilities between objects.

Visitor is a behavioral design pattern that lets you separate algorithms from the objects on which they operate.

#### Objects

- The **Visitor** interface declares a set of visiting methods that can take concrete elements of an object structure as arguments. These methods may have the same names if the program is written in a language that supports overloading, but the type of their parameters must be different.

- Each **Concrete Visitor** implements several versions of the same behaviors, tailored for different concrete element classes.

- The **Element** interface declares a method for “accepting” visitors. This method should have one parameter declared with the type of the visitor interface.

- Each **Concrete Element** must implement the acceptance method. The purpose of this method is to redirect the call to the proper visitor’s method corresponding to the current element class. Be aware that even if a base element class implements this method, all subclasses must still override this method in their own classes and call the appropriate method on the visitor object.

- The **Client** usually represents a collection or some other complex object (for example, a Composite tree). Usually, clients aren’t aware of all the concrete element classes because they work with objects from that collection via some abstract interface.

<img src="https://refactoring.guru/images/patterns/diagrams/visitor/structure-en-indexed.png" width="500" />

#### Application

- Use the Visitor when you need to perform an operation on all elements of a complex object structure (for example, an object tree).
- Use the Visitor to clean up the business logic of auxiliary behaviors.
- Use the pattern when a behavior makes sense only in some classes of a class hierarchy, but not in others.

#### Code

```typescript

// The element interface declares an `accept` method that takes
// the base visitor interface as an argument.
interface Shape is
    method move(x, y)
    method draw()
    method accept(v: Visitor)

// Each concrete element class must implement the `accept`
// method in such a way that it calls the visitor's method that
// corresponds to the element's class.
class Dot implements Shape is
    // ...

    // Note that we're calling `visitDot`, which matches the
    // current class name. This way we let the visitor know the
    // class of the element it works with.
    method accept(v: Visitor) is
        v.visitDot(this)

class Circle implements Shape is
    // ...
    method accept(v: Visitor) is
        v.visitCircle(this)

class Rectangle implements Shape is
    // ...
    method accept(v: Visitor) is
        v.visitRectangle(this)

class CompoundShape implements Shape is
    // ...
    method accept(v: Visitor) is
        v.visitCompoundShape(this)

// The Visitor interface declares a set of visiting methods that
// correspond to element classes. The signature of a visiting
// method lets the visitor identify the exact class of the
// element that it's dealing with.
interface Visitor is
    method visitDot(d: Dot)
    method visitCircle(c: Circle)
    method visitRectangle(r: Rectangle)
    method visitCompoundShape(cs: CompoundShape)

// Concrete visitors implement several versions of the same
// algorithm, which can work with all concrete element classes.
//
// You can experience the biggest benefit of the Visitor pattern
// when using it with a complex object structure such as a
// Composite tree. In this case, it might be helpful to store
// some intermediate state of the algorithm while executing the
// visitor's methods over various objects of the structure.
class XMLExportVisitor implements Visitor is
    method visitDot(d: Dot) is
        // Export the dot's ID and center coordinates.

    method visitCircle(c: Circle) is
        // Export the circle's ID, center coordinates and
        // radius.

    method visitRectangle(r: Rectangle) is
        // Export the rectangle's ID, left-top coordinates,
        // width and height.

    method visitCompoundShape(cs: CompoundShape) is
        // Export the shape's ID as well as the list of its
        // children's IDs.


// The client code can run visitor operations over any set of
// elements without figuring out their concrete classes. The
// accept operation directs a call to the appropriate operation
// in the visitor object.
class Application is
    field allShapes: array of Shapes

    method export() is
        exportVisitor = new XMLExportVisitor()

        foreach (shape in allShapes) do
            shape.accept(exportVisitor)

            // The visitor pattern allows the definition of new operations to the collection of objects without changing the structure of the objects themselves. This allows us to separate the class from the logic it implements.
// The extra operations can be encapsulated in a visitor object. The objects can have a visit method that accepts the visitor object. The visitor can then make the required changes and perform the operations on the object that received it. This allows the developers to make future extensions, extend the libraries/frameworks, etc.

class Visitor {
	visit(item) {
		//nada
	}
}

class BookVisitor extends Visitor {
	visit(book) {
		let cost = 0;

		if (book.getPrice() > 50) {
			cost = book.getPrice() * 0.5;
		} else {
			cost = book.getPrice();
		}

		console.log(
			"Book name: " +
				book.getName() +
				"\n" +
				"ID: " +
				book.getID() +
				"\n" +
				"cost: " +
				cost
		);

		return cost;
	}
}

class Book {
	constructor(id, name, price) {
		this.id = id;
		this.name = name;
		this.price = price;
	}

	getPrice() {
		return this.price;
	}

	getName() {
		return this.name;
	}

	getID() {
		return this.id;
	}

	accept(visitor) {
		return visitor.visit(this);
	}
}

let visitor = new BookVisitor();
let someBook = new Book("#1234", "lordOftheRings", 80);
someBook.accept(visitor);

```
