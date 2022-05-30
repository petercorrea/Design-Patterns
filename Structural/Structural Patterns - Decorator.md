---
attachments: [factory.png]
tags: [Design Patterns]
title: Structural Patterns - Decorator
created: "2021-05-08T17:34:49.986Z"
modified: "2021-05-08T17:39:37.249Z"
---

// DECORATOR
// Intent:
// add behaviors to objects by wrapping them inside an object that has said behavior
// Design:
// text

# Structural Patterns - Decorator

###### Design patterns enable us to refactor code to provide clarity and intent through structure. They allow us to apply general solutions to common problems. Structural patterns are concerned with how to assemble objects and classes into larger structures while keeping these structures flexible and efficient.

Decorator lets you attach new behaviors to objects by placing these objects inside special wrapper objects that contain the behaviors.

#### Objects

- **Component** declares the common interface for both wrappers and wrapped objects.
- **Concrete Component** is a class of objects being wrapped. It defines the basic behavior, which can be altered by decorators.
- **Base Decorator** class has a field for referencing a wrapped object. The field’s type should be declared as the component interface so it can contain both concrete components and decorators. The base decorator delegates all operations to the wrapped object.
- **Concrete Decorators** define extra behaviors that can be added to components dynamically. Concrete decorators override methods of the base decorator and execute their behavior either before or after calling the parent method.
- **Client** can wrap components in multiple layers of decorators, as long as it works with all objects via the component interface.

<img src="https://refactoring.guru/images/patterns/diagrams/decorator/structure-indexed.png" width="500" />

#### Application

- Use the Decorator pattern when you need to be able to assign extra behaviors to objects at runtime without breaking the code that uses these objects.
- Use the pattern when it’s awkward or not possible to extend an object’s behavior using inheritance.

#### Code

```typescript

// The component interface defines operations that can be
// altered by decorators.
interface DataSource is
    method writeData(data)
    method readData():data

// Concrete components provide default implementations for the
// operations. There might be several variations of these
// classes in a program.
class FileDataSource implements DataSource is
    constructor FileDataSource(filename) { ... }

    method writeData(data) is
        // Write data to file.

    method readData():data is
        // Read data from file.

// The base decorator class follows the same interface as the
// other components. The primary purpose of this class is to
// define the wrapping interface for all concrete decorators.
// The default implementation of the wrapping code might include
// a field for storing a wrapped component and the means to
// initialize it.
class DataSourceDecorator implements DataSource is
    protected field wrappee: DataSource

    constructor DataSourceDecorator(source: DataSource) is
        wrappee = source

    // The base decorator simply delegates all work to the
    // wrapped component. Extra behaviors can be added in
    // concrete decorators.
    method writeData(data) is
        wrappee.writeData(data)

    // Concrete decorators may call the parent implementation of
    // the operation instead of calling the wrapped object
    // directly. This approach simplifies extension of decorator
    // classes.
    method readData():data is
        return wrappee.readData()

// Concrete decorators must call methods on the wrapped object,
// but may add something of their own to the result. Decorators
// can execute the added behavior either before or after the
// call to a wrapped object.

class EncryptionDecorator extends DataSourceDecorator is
    method writeData(data) is
        // 1. Encrypt passed data.
        // 2. Pass encrypted data to the wrappee's writeData
        // method.

    method readData():data is
        // 1. Get data from the wrappee's readData method.
        // 2. Try to decrypt it if it's encrypted.
        // 3. Return the result.

// You can wrap objects in several layers of decorators.
class CompressionDecorator extends DataSourceDecorator is
    method writeData(data) is
        // 1. Compress passed data.
        // 2. Pass compressed data to the wrappee's writeData
        // method.

    method readData():data is
        // 1. Get data from the wrappee's readData method.
        // 2. Try to decompress it if it's compressed.
        // 3. Return the result.


// Option 1. A simple example of a decorator assembly.
class Application is
    method dumbUsageExample() is
        source = new FileDataSource("somefile.dat")
        source.writeData(salaryRecords)
        // The target file has been written with plain data.

        source = new CompressionDecorator(source)
        source.writeData(salaryRecords)
        // The target file has been written with compressed
        // data.

        source = new EncryptionDecorator(source)
        // The source variable now contains this:
        // Encryption > Compression > FileDataSource
        source.writeData(salaryRecords)
        // The file has been written with compressed and
        // encrypted data.

// Option 2. Client code that uses an external data source.
// SalaryManager objects neither know nor care about data
// storage specifics. They work with a pre-configured data
// source received from the app configurator.
class SalaryManager is
    field source: DataSource

    constructor SalaryManager(source: DataSource) { ... }

    method load() is
        return source.readData()

    method save() is
        source.writeData(salaryRecords)
    // ...Other useful methods...


// The app can assemble different stacks of decorators at
// runtime, depending on the configuration or environment.
class ApplicationConfigurator is
    method configurationExample() is
        source = new FileDataSource("salary.dat")
        if (enabledEncryption)
            source = new EncryptionDecorator(source)
        if (enabledCompression)
            source = new CompressionDecorator(source)

        logger = new SalaryManager(source)
        salary = logger.load()
    // ...

    // The decorator pattern focuses on adding properties, functionalities, and behavior to existing classes dynamically.
// The decorator pattern lets you modify the code without changing the original class.

class Burger {
	constructor(meat, price) {
		this.meat = meat;
		this.price = price;
	}

	orderPlaced() {
		console.log(
			`Please wait for your ${this.meat} burger. Pay ${this.price}.`
		);
	}
}

function addVeggies(burgerOrder) {
	burgerOrder.addLetteuce = true;
	burgerOrder.addTomatos = true;
	burgerOrder.price += 0.5;

	burgerOrder.allVeggies = function () {
		console.log(
			`
			Your burger has:
			${burgerOrder.addLetteuce ? "letteuce," : ""}
			${burgerOrder.addLetteuce ? "tomatoes" : ""}
			.`
		);
	};

	return burgerOrder;
}

let myBurger = new Burger("Chicken", 5.0);
let myBurgerWithVeggies = addVeggies(myBurger);
console.log(myBurgerWithVeggies);

```
