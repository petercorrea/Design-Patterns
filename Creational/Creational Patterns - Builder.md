---
attachments: [factory.png]
tags: [Design Patterns]
title: Creational Patterns - Builder
created: "2021-05-07T20:26:46.690Z"
modified: "2021-05-08T17:39:37.073Z"
---

// BUILDER
// Intent:
// construct complex objects incrementally
// Design:
//

# Creational Patterns - Builder

###### Design patterns enable us to refactor code to provide clarity and intent through structure. They allow us to apply general solutions to common problems. Creational patterns are concerned with object creation mechanisms.

Builder lets you construct complex objects step by step and allows you to produce different representations of an object using the same construction code.

#### Objects

- **Abstract Builder** interface declares product construction steps that are common to all types of builders.
- **Concrete Builder** provide different implementations of the construction steps. Concrete builders may produce products that don’t follow the common interface.
- **Product** Products constructed by different concrete builders.
- **Director** defines the order in which to call construction steps.
- **Client** the client passes the builder object to the director. You can use a different builder each time you produce something with the director.

<img src="https://refactoring.guru/images/patterns/diagrams/builder/structure-indexed.png" width="500" />

#### Application

- Use the Builder pattern when you want your code to be able to create different representations of some product (e.g., manual and automatic sport cars).
- Use the Builder to construct Composite trees or other complex objects.

#### Code

```typescript

// Products with two different interfaces
class Car is
class Manual is

// The baseclass provides interface
interface Builder is
    abstract function reset()
    abstract function setSeats()
    abstract function setEngine()
    abstract function setTripComputer()
    abstract function setGPS()

// The subclass implements
class CarBuilder implements Builder is
    var car:Car
    constructor CarBuilder() is
        this.reset()
    function reset() is
        this.car = new Car()
    function setSeats() is
    function setEngine() is
    function setTripComputer() is
    function setGPS() is
    function getProduct():Car is

// The other subclasses folow the interface but implements something different
class ManualBuilder implements Builder is
    var manual:Manual
    constructor ManualBuilder() is
        this.reset()
    function reset() is
        this.manual = new Manual()
    function setSeats() is
    function setEngine() is
    function setTripComputer() is
    function setGPS() is
    function getProduct():Manual is

// The director works with any builder instance that the client code passes to it
// The director is only responsible for executing the building steps in a particular sequence
class Director is
    var builder:Builder
    function setBuilder(builder:Builder)
        this.builder = builder
    function constructSportsCar(builder: Builder) is
        builder.reset()
        builder.setSeats(2)
        builder.setEngine(new SportEngine())
        builder.setTripComputer(true)
        builder.setGPS(true)
    function constructSUV(builder: Builder) is
        builder.reset()
        builder.setSeats(4)
        builder.setEngine(new SUVEngine())
        builder.setTripComputer(false)
        builder.setGPS(false)

class Application is
    function makeCar() is
        // instantiate the director
        var director = new Director()
        // instantiate different builder objects
        var manualBuilder: ManualBuilder = new ManualBuilder()
        var builder: CarBuilder = new CarBuilder()
        // build the car manual book and the actual car
        director.constructSportsCar(manualBuilder)
        director.constructSportsCar(builder)
        // retrieve the products
        var manual: Manual = manualBuilder.getProduct()
        var car: Car = builder.getProduct()
```
