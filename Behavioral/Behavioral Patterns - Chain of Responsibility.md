---
attachments: [factory.png]
tags: [Design Patterns]
title: Behavioral Patterns - Chain of Responsibility
created: "2021-05-08T17:48:22.902Z"
modified: "2021-05-08T17:54:29.835Z"
---

// CHAIN OF RESPONSIBILITY
// Intent:
// pass requests along a chain of handlers
// Design:
// text

# Behavioral Patterns - Chain of Responsibility

###### Design patterns enable us to refactor code to provide clarity and intent through structure. They allow us to apply general solutions to common problems. Behavioral patterns are concerned with algorithms and the assignment of responsibilities between objects.

Chain of Responsibility is a behavioral design pattern that lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.

#### Objects

- The **Handler** declares the interface, common for all concrete handlers. It usually contains just a single method for handling requests, but sometimes it may also have another method for setting the next handler on the chain.

- The **Base Handler** is an optional class where you can put the boilerplate code that’s common to all handler classes.

Usually, this class defines a field for storing a reference to the next handler. The clients can build a chain by passing a handler to the constructor or setter of the previous handler. The class may also implement the default handling behavior: it can pass execution to the next handler after checking for its existence.

- **Concrete Handlers** contain the actual code for processing requests. Upon receiving a request, each handler must decide whether to process it and, additionally, whether to pass it along the chain.

Handlers are usually self-contained and immutable, accepting all necessary data just once via the constructor.

- The **Client** may compose chains just once or compose them dynamically, depending on the application’s logic. Note that a request can be sent to any handler in the chain—it doesn’t have to be the first one.

<img src="https://refactoring.guru/images/patterns/diagrams/chain-of-responsibility/structure-indexed.png" width="500" />

#### Application

- Use the Chain of Responsibility pattern when your program is expected to process different kinds of requests in various ways, but the exact types of requests and their sequences are unknown beforehand.
- Use the pattern when it’s essential to execute several handlers in a particular order.
- Use the CoR pattern when the set of handlers and their order are supposed to change at runtime.

#### Code

```typescript
// The handler interface declares a method for building a chain
// of handlers. It also declares a method for executing a
// request.
interface ComponentWithContextualHelp is
    method showHelp()


// The base class for simple components.
abstract class Component implements ComponentWithContextualHelp is
    field tooltipText: string

    // The component's container acts as the next link in the
    // chain of handlers.
    protected field container: Container

    // The component shows a tooltip if there's help text
    // assigned to it. Otherwise it forwards the call to the
    // container, if it exists.
    method showHelp() is
        if (tooltipText != null)
            // Show tooltip.
        else
            container.showHelp()


// Containers can contain both simple components and other
// containers as children. The chain relationships are
// established here. The class inherits showHelp behavior from
// its parent.
abstract class Container extends Component is
    protected field children: array of Component

    method add(child) is
        children.add(child)
        child.container = this


// Primitive components may be fine with default help
// implementation...
class Button extends Component is
    // ...

// But complex components may override the default
// implementation. If the help text can't be provided in a new
// way, the component can always call the base implementation
// (see Component class).
class Panel extends Container is
    field modalHelpText: string

    method showHelp() is
        if (modalHelpText != null)
            // Show a modal window with the help text.
        else
            super.showHelp()

// ...same as above...
class Dialog extends Container is
    field wikiPageURL: string

    method showHelp() is
        if (wikiPageURL != null)
            // Open the wiki help page.
        else
            super.showHelp()


// Client code.
class Application is
    // Every application configures the chain differently.
    method createUI() is
        dialog = new Dialog("Budget Reports")
        dialog.wikiPageURL = "http://..."
        panel = new Panel(0, 0, 400, 800)
        panel.modalHelpText = "This panel does..."
        ok = new Button(250, 760, 50, 20, "OK")
        ok.tooltipText = "This is an OK button that..."
        cancel = new Button(320, 760, 50, 20, "Cancel")
        // ...
        panel.add(ok)
        panel.add(cancel)
        dialog.add(panel)

    // Imagine what happens here.
    method onF1KeyPress() is
        component = this.getComponentAtMouseCoords()
        component.showHelp()
```

// The chain of responsibility pattern allows a request sent by a client to be received by more than one object.

```typescript
class HandlerChain {
  setNextObj(nextObj) {
    this.nextObjInChain = nextObj;
  }

  processMultiple(req) {
    console.log("No multiple for: " + req.getMultiple());
  }
}

class Multiple {
  constructor(multiple) {
    this.multiple = multiple;
  }

  getMultiple() {
    return this.multiple;
  }
}

class MultipleofTwoHandler extends HandlerChain {
  constructor() {
    super();
    this.nextObjInChain = new HandlerChain();
  }

  processMultiple(req) {
    if (req.getMultiple() % 2 == 0) {
      console.log("Multiple of 2: " + req.getMultiple());
    } else {
      this.nextObjInChain.processMultiple(req);
    }
  }
}

class MultipleofThreeHandler extends HandlerChain {
  constructor() {
    super();
    this.nextObjInChain = new HandlerChain();
  }

  processMultiple(req) {
    if (req.getMultiple() % 3 == 0) {
      console.log("Multiple of 3: " + req.getMultiple());
    } else {
      this.nextObjInChain.processMultiple(req);
    }
  }
}

class MultipleofFiveHandler extends HandlerChain {
  constructor() {
    super();
    this.nextObjInChain = new HandlerChain();
  }

  processMultiple(req) {
    if (req.getMultiple() % 5 == 0) {
      console.log("Multiple of 5: " + req.getMultiple());
    } else {
      this.nextObjInChain.processMultiple(req);
    }
  }
}

let c1 = new MultipleofTwoHandler();
let c2 = new MultipleofThreeHandler();
let c3 = new MultipleofFiveHandler();

c1.setNextObj(c2);
c2.setNextObj(c3);

c1.processMultiple(new Multiple(5));
c1.processMultiple(new Multiple(11));
c1.processMultiple(new Multiple(4));
c1.processMultiple(new Multiple(9));
```
