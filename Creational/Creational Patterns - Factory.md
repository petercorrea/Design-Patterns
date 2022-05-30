# Creational Patterns - Factory

## Concept

The Factory pattern allows classes to defer instantiation to subclasses. This pattern allows us to separate the instantiation of concrete products and their public API. Future extension is done by adding another concrete product with it's implementation details and a concrete factory to provide initialization details.

## Objects

- **Abstract Product** declares an interface for all related products
- **Concrete Product** provides implementation
- **Abstract Factory** declares an abstract constructor that returns abstract product, stores shared logic and provides a public API
- **Concrete Factory** extends abstract factory and implements constructor to return concrete product

## Application

- when you don’t know beforehand the exact types and dependencies of the objects your code should work with
- Open/Closed Principle: You can introduce new types of products without breaking existing client code.
- Single Responsibility Principle: The separation of instantiation and implementation makes the code easier to maintain.

### Pseudocode

The problem: Let's assume we're playing a Grand Theft Auto style game, where our player can
obtain and drive a vehicle. We do not know in advance what type of vehicle it may be.

```typescript

// abstract product
interface Engine {
    abstract function drive() {}
}

// concrete products will implement subclass details
class CarEngine extends Engine {
    function drive() {}
}

class BoatEngine extends Engine {
    function drive() {}
}

// abstract factory provide a public API
class Vehicle {
    // let subclasses implement instantiation
    abstract function createEngine(): Engine {}

    // provide a public API
    function drive() {
        // abstract constructor is called
        var engine: Engine = createEngine()
        // internal API
        engine.drive()
    }
}

// concrete factory will implement the instantiation
class Car extends Vehicle {
    function createEngine(): Engine {
        return new CarEngine()
    }
}

class Boat extends Vehicle {
    function createEngine(): Engine {
        return new BoatEngine()
    }
}

class Application {
    function initialize() {
        // subclasses decide how to instantiate
        // myVehicle is a concrete factory,
        // which returns a concrete object
        var myVehicle: Vehicle

        if (Global.vehicleType == "Car")
            myVehicle = new Car()
        else if (Global.vehicleType == "Boat")
            myVehicle = new Boat()
        else
            throw new Exception()
    }

    function main() {
        // initialize the concrete object
        this.initialize()
        // use the concrete object via the public API
        myVehicle.drive()
    }
}
```
