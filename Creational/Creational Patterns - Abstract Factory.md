---
attachments: [factory.png]
tags: [Design Patterns]
title: Creational Patterns - Abstract Factory
created: "2021-05-07T20:25:08.255Z"
modified: "2021-05-08T17:39:37.039Z"
---

// ABSTRACT FACTORY
// Intent:
// produce families of related objects without specifying their concrete classes
// Design:
// text

# Creational Patterns - Abstract Factory

###### Design patterns enable us to refactor code to provide clarity and intent through structure. They allow us to apply general solutions to common problems. Creational patterns are concerned with object creation mechanisms.

Abstract Factory that lets you produce families of related objects without specifying their concrete classes.

#### Objects

- **Abstract Product** declares an interface for all related products
- **Concrete Product** provides implementation
- **Abstract Factory** declares an interface with methods for creating each abstract products
- **Concrete Factory** implement creation methods
- **Client** the Client communicates with objects via their abstract interfaces. Signatures of Concrete factories constructors must return corresponding abstract products. This way the client code that uses a factory doesn’t get coupled to the specific variant of the product it gets from a factory.

<img src="https://refactoring.guru/images/patterns/diagrams/abstract-factory/structure-indexed.png" width="500" />

#### Application

- Use the Abstract Factory when your code needs to work with various families of related products, but you don’t want it to depend on the concrete classes of those products—they might be unknown beforehand or you simply want to allow for future extensibility.
- The Abstract Factory provides you with an interface for creating objects from each class of the product family. As long as your code creates objects via this interface, you don’t have to worry about creating the wrong variant of a product which doesn’t match the products already created by your app.

#### Code

```typescript

// abstract product provides interface
interface Button is
    abstract function paint()

interface Checkbox is
    abstract function paint()

// concrete products implement
class WinButton implements Button is
    function paint() is

class MacButton implements Button is
    function paint() is

class WinCheckbox implements Checkbox is
    function paint() is

class MacCheckbox implements Checkbox is
    function paint() is



// abstract factory declares an interface with methods that return abstract products
interface GUIFactory is
    abstract function createButton():Button
    abstract function createCheckbox():Checkbox

// concrete factories produce concrete products
// signatures of the concrete factory's methods return an abstract product, while inside the method a concrete product is instantiated
class WinFactory implements GUIFactory is
    function createButton():Button is
        return new WinButton()
    function createCheckbox():Checkbox is
        return new WinCheckbox()

class MacFactory implements GUIFactory is
    function createButton():Button is
        return new MacButton()
    function createCheckbox():Checkbox is
        return new MacCheckbox()



// The client only interacts with abstract factories and abstract products
// This lets you pass any factory or product subclass to the client without breaking it
class Application is
    var factory: GUIFactory
    var button: Button
    constructor Application(factory: GUIFactory) is
        this.factory = factory
    function createUI() is
        this.button = factory.createButton()
    function paint() is
        button.paint()



// The application on runtime will create one of the concrete factories
class ApplicationConfigurator is
    function main() is
        var factory: GUIFactory
        var config = readApplicationConfigFile()
        if (config.OS == "Windows")
            factory = new WinFactory()
        else if (config.OS == "Mac")
            factory = new MacFactory()
        else
            throw new Exception()
        // pass the factory into the client
        var app = new Application(factory)
        app.createUI()
        app.paint()
```
