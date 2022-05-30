---
attachments: [factory.png]
tags: [Design Patterns]
title: Structural Patterns - Flyweight
created: "2021-05-08T17:43:02.409Z"
modified: "2021-05-08T17:44:56.003Z"
---

// FLYWEIGHT
// Intent:
// allows more objects to exist in ram by sharing common parts of state between objects
// Design:
// text

# Structural Patterns - Flyweight

###### Design patterns enable us to refactor code to provide clarity and intent through structure. They allow us to apply general solutions to common problems. Structural patterns are concerned with how to assemble objects and classes into larger structures while keeping these structures flexible and efficient.

Flyweight lets you fit more objects into the available amount of RAM by sharing common parts of state between multiple objects instead of keeping all of the data in each object.

#### Objects

The Flyweight pattern is merely an optimization. Before applying it, make sure your program does have the RAM consumption problem related to having a massive number of similar objects in memory at the same time. Make sure that this problem can’t be solved in any other meaningful way.

- **Flyweight** class contains the portion of the original object’s state that can be shared between multiple objects. The same flyweight object can be used in many different contexts. The state stored inside a flyweight is called intrinsic. The state passed to the flyweight’s methods is called extrinsic.

- **Context** class contains the extrinsic state, unique across all original objects. When a context is paired with one of the flyweight objects, it represents the full state of the original object.

Usually, the behavior of the original object remains in the flyweight class. In this case, whoever calls a flyweight’s method must also pass appropriate bits of the extrinsic state into the method’s parameters. On the other hand, the behavior can be moved to the context class, which would use the linked flyweight merely as a data object.

- **Client** calculates or stores the extrinsic state of flyweights. From the client’s perspective, a flyweight is a template object which can be configured at runtime by passing some contextual data into parameters of its methods.

- **Flyweight Factory** manages a pool of existing flyweights. With the factory, clients don’t create flyweights directly. Instead, they call the factory, passing it bits of the intrinsic state of the desired flyweight. The factory looks over previously created flyweights and either returns an existing one that matches search criteria or creates a new one if nothing is found.

<img src="https://refactoring.guru/images/patterns/diagrams/flyweight/structure-indexed.png" width="500" />

#### Application

- Use the Flyweight pattern only when your program must support a huge number of objects which barely fit into available RAM.

#### Code

```typescript

// The flyweight class contains a portion of the state of a
// tree. These fields store values that are unique for each
// particular tree. For instance, you won't find here the tree
// coordinates. But the texture and colors shared between many
// trees are here. Since this data is usually BIG, you'd waste a
// lot of memory by keeping it in each tree object. Instead, we
// can extract texture, color and other repeating data into a
// separate object which lots of individual tree objects can
// reference.
class TreeType is
    field name
    field color
    field texture
    constructor TreeType(name, color, texture) { ... }
    method draw(canvas, x, y) is
        // 1. Create a bitmap of a given type, color & texture.
        // 2. Draw the bitmap on the canvas at X and Y coords.

// Flyweight factory decides whether to re-use existing
// flyweight or to create a new object.
class TreeFactory is
    static field treeTypes: collection of tree types
    static method getTreeType(name, color, texture) is
        type = treeTypes.find(name, color, texture)
        if (type == null)
            type = new TreeType(name, color, texture)
            treeTypes.add(type)
        return type

// The contextual object contains the extrinsic part of the tree
// state. An application can create billions of these since they
// are pretty small: just two integer coordinates and one
// reference field.
class Tree is
    field x,y
    field type: TreeType
    constructor Tree(x, y, type) { ... }
    method draw(canvas) is
        type.draw(canvas, this.x, this.y)

// The Tree and the Forest classes are the flyweight's clients.
// You can merge them if you don't plan to develop the Tree
// class any further.
class Forest is
    field trees: collection of Trees

    method plantTree(x, y, name, color, texture) is
        type = TreeFactory.getTreeType(name, color, texture)
        tree = new Tree(x, y, type)
        trees.add(tree)

    method draw(canvas) is
        foreach (tree in trees) do
            tree.draw(canvas)

            // It is a structural pattern that focuses on how related objects share data.
// This pattern takes the common data structures/objects that are used by a lot of objects and stores them in an external object (flyweight) for sharing.

class CodeFile {
	constructor(codefileName) {
		this.codefileName = codefileName;
	}
}

class Formatter {
	format(codefile) {
		//nada
	}
}

class PythonFormatter extends Formatter {
	constructor() {
		super();
		console.log("Python formatter instance created");
	}

	format(codefileName) {
		console.log(`Formatting the Python file...`);
	}
}

class JavaFormatter extends Formatter {
	constructor() {
		super();
		console.log("Java formatter instance created");
	}

	format(codefileName) {
		console.log("Formatting the Java file...");
	}
}

class FormatterFactory {
	constructor() {
		this.formatterMap = new Map();
	}

	createFormatter(formatterType) {
		let formatter = this.formatterMap.get(formatterType);

		if (formatter == null) {
			if (formatterType == "Python") {
				formatter = new PythonFormatter();
			} else if (formatterType == "Java") {
				formatter = new JavaFormatter();
			}

			this.formatterMap.set(formatterType, formatter);
		}

		return formatter;
	}
}

let someCodefile = new CodeFile("helloWorld.py");
let anotherCodefile = new CodeFile("test.py");
let someJavafile = new CodeFile("file.java");
let someFormatterFactory = new FormatterFactory();

let somePythonFormatter = someFormatterFactory.createFormatter("Python");
somePythonFormatter.format(someCodefile.codefileName);

let anotherPythonFormatter = someFormatterFactory.createFormatter("Python");
anotherPythonFormatter.format(anotherCodefile.codefileName);

let javaFormatter = someFormatterFactory.createFormatter("Java");
javaFormatter.format(someJavafile);

```
