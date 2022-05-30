---
attachments: [factory.png]
tags: [Design Patterns]
title: Structural Patterns - Composite
created: "2021-05-08T17:26:25.350Z"
modified: "2021-05-08T17:39:37.224Z"
---

// COMPOSITE
// Intent:
// compose objects into trees with provided functionalities
// Design:
// a client interacts through a common interface provided by the leaf and composite

# Structural Patterns - Composite

###### Design patterns enable us to refactor code to provide clarity and intent through structure. They allow us to apply general solutions to common problems. Structural patterns are concerned with how to assemble objects and classes into larger structures while keeping these structures flexible and efficient.

Composite lets you compose objects into tree structures and then work with these structures as if they were individual objects.

#### Objects

- **Component** provides interface
- **Leaf** is a basic element of a tree that doesn’t have sub-elements.
- **Composite** is an element that has sub-elements: leaves or other containers. A container doesn’t know the concrete classes of its children. It works with all sub-elements only via the component interface. Upon receiving a request, a container delegates the work to its sub-elements, processes intermediate results and then returns the final result to the client.
- **Client** works with all elements through the component interface. As a result, the client can work in the same way with both simple or complex elements of the tree.

<img src="https://refactoring.guru/images/patterns/diagrams/composite/structure-en-indexed.png" width="500" />

#### Application

- when you have to implement a tree-like object structure.
- when you want the client code to treat both simple and complex elements uniformly.

#### Code

```typescript
// The component interface declares common operations for both
// simple and complex objects of a composition.
interface Graphic is
    method move(x, y)
    method draw()

// The leaf class represents end objects of a composition. A
// leaf object can't have any sub-objects. Usually, it's leaf
// objects that do the actual work, while composite objects only
// delegate to their sub-components.
class Dot implements Graphic is
    field x, y

    constructor Dot(x, y) { ... }

    method move(x, y) is
        this.x += x, this.y += y

    method draw() is
        // Draw a dot at X and Y.

// All component classes can extend other components.
class Circle extends Dot is
    field radius

    constructor Circle(x, y, radius) { ... }

    method draw() is
        // Draw a circle at X and Y with radius R.

// The composite class represents complex components that may
// have children. Composite objects usually delegate the actual
// work to their children and then "sum up" the result.
class CompoundGraphic implements Graphic is
    field children: array of Graphic

    // A composite object can add or remove other components
    // (both simple or complex) to or from its child list.
    method add(child: Graphic) is
        // Add a child to the array of children.

    method remove(child: Graphic) is
        // Remove a child from the array of children.

    method move(x, y) is
        foreach (child in children) do
            child.move(x, y)

// A composite executes its primary logic in a particular
    // way. It traverses recursively through all its children,
    // collecting and summing up their results. Since the
    // composite's children pass these calls to their own
    // children and so forth, the whole object tree is traversed
    // as a result.
    method draw() is
        // 1. For each child component:
        //     - Draw the component.
        //     - Update the bounding rectangle.
        // 2. Draw a dashed rectangle using the bounding
        // coordinates.


// The client code works with all the components via their base
// interface. This way the client code can support simple leaf
// components as well as complex composites.
class ImageEditor is
    field all: CompoundGraphic

    method load() is
        all = new CompoundGraphic()
        all.add(new Dot(1, 2))
        all.add(new Circle(5, 3, 10))
        // ...

    // Combine selected components into one complex composite
    // component.
    method groupSelected(components: array of Graphic) is
        group = new CompoundGraphic()
        foreach (component in components) do
            group.add(component)
            all.remove(component)
        all.add(group)
        // All components will be drawn.
        all.draw()

        // The composite pattern is used to structure objects in a tree-like hierarchy.
// This pattern allows the client to work with these components uniformly,
// that is, a single object can be treated exactly how a group of objects of the same instance is treated.

// This pattern allows the formation of deeply-nested structures.
// If a leaf object receives the request sent by the client, it will handle it. However, if the recipient is
// composed of children, the request is forwarded to the child components.

// The composite pattern consists of components, leaves, and composites.
// A component is an abstract class which can be used as either a leaf object or a composite object.
// It contains methods for managing child objects like add, remove and getChild and
// methods specific to all components. A leaf is a subclass of a component that has no child
// objects and defines behavior for an individual object. A composite is a subclass of a component
// that stores child components and defines the behavior for operating on objects that have children.

// Component
class Employee {
	constructor(name, position, progress) {
		this.name = name;
		this.position = position;
		this.progress = progress;
	}

	getProgress() {
		// nada
	}
}

// Leaf
class Developers extends Employee {
	constructor(name, position, progress) {
		super(name, position, progress);
	}

	getProgress() {
		return this.progress;
	}
}

// Leaf
class FreeLanceDev extends Employee {
	constructor(name, position, progress) {
		super(name, position, progress);
	}

	getProgress() {
		return this.progress();
	}
}

// Composite
class TeamLeadDev extends Employee {
	constructor(name, position) {
		super(name, position);
		this.teamMembers = [];
	}

	addMember(employee) {
		this.teamMembers.push(employee);
	}

	removeMember(employee) {
		for (let i = 0; i < this.teamMembers.length; i++) {
			if (this.teamMembers[i] == employee) {
				this.teamMembers.splice(i, 1);
			}
		}

		return this.teamMembers;
	}

	getProgress() {
		for (let i = 0; i < this.teamMembers.length; i++) {
			console.log(this.teamMembers[i].getProgress());
		}
	}

	showTeam() {
		for (let i = 0; i < this.teamMembers.length; i++) {
			console.log(this.teamMembers[i].name);
		}
	}
}

const seniorDev = new Developers("Rachel", "Senior Developer", "60%");
const juniorDev = new Developers("Joey", "Junior Developer", "50%");
const teamLead = new TeamLeadDev("Regina", "Dev Team Lead", "90%");
teamLead.addMember(seniorDev);
teamLead.addMember(juniorDev);
console.log("Team members list:");
teamLead.showTeam();
console.log("Get Team members progress:");
teamLead.getProgress();
console.log("Removing Rachel from team:");
teamLead.removeMember(seniorDev);
console.log("Updated team members list:");
teamLead.showTeam();
const freelanceDev = new Developers("Ross", "Free Lancer", "80%");
console.log("Get freelance developer's progress:");
console.log(freelanceDev.getProgress());

```
