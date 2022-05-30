---
attachments: [factory.png]
tags: [Design Patterns]
title: Structural Patterns - Adapter
created: "2021-05-08T05:52:35.239Z"
modified: "2021-05-08T17:39:37.174Z"
---

// ADAPTER
// Intent:
// allows objects with incompatible interfaces to collaborate
// Design:
// change the interface of an existing service object by wrapping the service object the client is attempting to use

# Structural Patterns - Adapter

###### Design patterns enable us to refactor code to provide clarity and intent through structure. They allow us to apply general solutions to common problems. Structural patterns are concerned with how to assemble objects and classes into larger structures while keeping these structures flexible and efficient.

Adapter allows objects with incompatible interfaces to collaborate.

#### Objects

- **Client** contains existing business logic. The client code doesn’t get coupled to the concrete adapter class as long as it works with the adapter via the client interface. Now we can introduce new types of adapters into the program without breaking the existing client code. This can be useful when the interface of the service class gets changed or replaced: you can just create a new adapter class without changing the client code.
- **Client Interface** provides an interface.
- **Adapter** a class that’s able to work with both the client and the service: it implements the client interface, while wrapping the service object. The adapter receives calls from the client via the adapter interface and translates them into calls to the wrapped service object in a format it can understand.
- **Service** a useful class (usually 3rd-party or legacy). The client can’t use this class directly because it has an incompatible interface.

This implementation uses the object composition principle: the adapter implements the interface of one object and wraps the other one. It can be implemented in all popular programming languages.
<img src="https://refactoring.guru/images/patterns/diagrams/adapter/structure-object-adapter-2x.png" width="500" />

#### Application

- Use the Adapter class when you want to use some existing class, but its interface isn’t compatible with the rest of your code.
- Use the pattern when you want to reuse several existing subclasses that lack some common functionality that can’t be added to the superclass.

#### Code

```typescript
// Say you have two classes with compatible interfaces: RoundHole and RoundPeg.
class RoundHole is
    constructor RoundHole(radius) is
    function getRadius() is
    function fits(peg: RoundPeg) is
        return this.getRadius() >= peg.getRadius()

class RoundPeg is
    constructor RoundPeg(radius) is
    function getRadius() is

// But there's an incompatible class: SquarePeg.
class SquarePeg is
    constructor SquarePeg(width) is
    function getWidth() is


// An adapter class extends the RoundPeg class to let the adapter objects act
// as round pegs...i.e., lets you fit square pegs into round holes.
class SquarePegAdapter extends RoundPeg is
    // In reality, the adapter contains an instance of the SquarePeg class.
    var peg: SquarePeg
    constructor SquarePegAdapter(peg: SquarePeg) is
        this.peg = peg
    function getRadius() is
        // The adapter pretends that it's a round peg
        return peg.getWidth() * Math.sqrt(2) / 2


// Somewhere in client code.
var hole = new RoundHole(5)
var rpeg = new RoundPeg(5)
hole.fits(rpeg) // true

var small_sqpeg = new SquarePeg(5)
var large_sqpeg = new SquarePeg(10)
hole.fits(small_sqpeg) // this won't compile (incompatible types)

var small_sqpeg_adapter = new SquarePegAdapter(small_sqpeg)
var large_sqpeg_adapter = new SquarePegAdapter(large_sqpeg)
hole.fits(small_sqpeg_adapter) // true
hole.fits(large_sqpeg_adapter) // false

// The adapter pattern allows classes that have different interfaces (properties/methods of an object) to work together. It translates the interface for a class to make it compatible with another class.
class OldApi {
	constructor() {
		this.attach = function () {
			console.log("This is an old api method.");
		};
	}
}

class NewApi {
	constructor() {
		this.attach = function () {
			console.log("This is the new api method");
		};
	}
}

class ApiAdapter extends OldApi {
	constructor(someNewApi) {
		super();
		this.attach = function () {
			someNewApi.attach();
		};
	}
}

let newApiObj = new NewApi();
let adapter = new ApiAdapter(newApiObj);
adapter.attach();

// Another Example
class AnotherAdapter {
	constructor() {
		this.someNewApi = new NewApi();

		this.attach = function () {
			return this.someNewApi.attach();
		};
	}
}

let anotherAdapter = new AnotherAdapter();
anotherAdapter.attach();


```
