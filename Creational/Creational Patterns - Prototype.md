---
attachments: [factory.png]
tags: [Design Patterns]
title: Creational Patterns - Prototype
created: "2021-05-07T20:27:04.702Z"
modified: "2021-05-08T17:39:37.127Z"
---

// PROTOTYPE
// Intent:
// copy existing objects without making your code dependent on their classes
// Design:
// text

# Creational Patterns - Prototype

###### Design patterns enable us to refactor code to provide clarity and intent through structure. They allow us to apply general solutions to common problems. Creational patterns are concerned with object creation mechanisms.

Prototypes let you copy existing objects without making your code dependent on their classes.

#### Objects

- **Prototype** interface declares the cloning methods.
- **Conrete Prototype** implements the cloning method
- **Client** can produce a copy of any object that follows the prototype interface.
  ![alt text](https://refactoring.guru/images/patterns/diagrams/prototype/example.png)

<img src="https://refactoring.guru/images/patterns/diagrams/prototype/example.png" width="500" />

#### Application

- Use the Prototype pattern when your code shouldn’t depend on the concrete classes of objects that you need to copy.
- Use the pattern when you want to reduce the number of subclasses that only differ in the way they initialize their respective objects. Somebody could have created these subclasses to be able to create objects with a specific configuration.

#### Code

```typescript
// Base prototype.
abstract class Shape is
    var X: int
    var Y: int
    var color: string
    // normal constructor
    constructor Shape() is
    // The prototype constructor.
    // A fresh object is initialized with values from the existing object.
    constructor Shape(source: Shape) is
        this()
        this.X = source.X
        this.Y = source.Y
        this.color = source.color
    // The clone operation returns one of the Shape subclasses.
    abstract function clone():Shape


// Concrete prototype. The cloning method creates a new object
// and passes it to the constructor. Until the constructor is
// finished, it has a reference to a fresh clone.
class Rectangle extends Shape is
    var width: int
    var height: intn
    constructor Rectangle(source: Rectangle) is
        // A parent constructor call is needed to copy private
        // fields defined in the parent class.
        super(source)
        this.width = source.width
        this.height = source.height
    function clone():Shape is
        return new Rectangle(this)


class Circle extends Shape is
    var radius: int
    constructor Circle(source: Circle) is
        super(source)
        this.radius = source.radius
    function clone():Shape is
        return new Circle(this)


// Somewhere in the client code.
class Application is
    var shapes: Array = new Array<Shapes>
    constructor Application() is
        Circle circle = new Circle()
        circle.X = 10
        circle.Y = 10
        circle.radius = 20
        shapes.add(circle)

        Circle anotherCircle = circle.clone()
        shapes.add(anotherCircle)
        // The `anotherCircle` variable contains an exact copy
        // of the `circle` object.

        Rectangle rectangle = new Rectangle()
        rectangle.width = 10
        rectangle.height = 20
        shapes.add(rectangle)

    function businessLogic() is
        var shapesCopy: Array = new Array<Shapes>
        for shape in shapes do
            shapesCopy.add(shape.clone())

        // We don't know the exact elements in the
        // shapes array, only that they are all shapes.
        // But thanks to polymorphism, when we call the
        // `clone` method on a shape the program checks its real
        // class and runs the appropriate clone method defined
        // in that class. That's why we get proper clones
        // instead of a set of simple Shape objects.
        // The `shapesCopy` array contains exact copies of the
        // `shape` array's children.
```
