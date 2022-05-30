---
attachments: [factory.png]
tags: [Design Patterns]
title: Behavioral Patterns - Mediator
created: "2021-05-08T17:51:02.261Z"
modified: "2021-05-08T18:02:05.013Z"
---

// MEDIATOR
// Intent:
// forces objects to collaborate only through a middleman
// Design:
// text

# Behavioral Patterns - Mediator

###### Design patterns enable us to refactor code to provide clarity and intent through structure. They allow us to apply general solutions to common problems. Behavioral patterns are concerned with algorithms and the assignment of responsibilities between objects.

Mediator is a behavioral design pattern that lets you reduce chaotic dependencies between objects. The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.

#### Objects

- **Components** are various classes that contain some business logic. Each component has a reference to a mediator, declared with the type of the mediator interface. The component isn’t aware of the actual class of the mediator, so you can reuse the component in other programs by linking it to a different mediator.

- The **Mediator** interface declares methods of communication with components, which usually include just a single notification method. Components may pass any context as arguments of this method, including their own objects, but only in such a way that no coupling occurs between a receiving component and the sender’s class.

- **Concrete Mediators** encapsulate relations between various components. Concrete mediators often keep references to all components they manage and sometimes even manage their lifecycle.

- **Components** must not be aware of other components. If something important happens within or to a component, it must only notify the mediator. When the mediator receives the notification, it can easily identify the sender, which might be just enough to decide what component should be triggered in return.

From a component’s perspective, it all looks like a total black box. The sender doesn’t know who’ll end up handling its request, and the receiver doesn’t know who sent the request in the first place.

<img src="https://refactoring.guru/images/patterns/diagrams/mediator/structure-indexed.png" width="500" />

#### Application

- Use the Mediator pattern when it’s hard to change some of the classes because they are tightly coupled to a bunch of other classes.
- Use the pattern when you can’t reuse a component in a different program because it’s too dependent on other components.
- Use the Mediator when you find yourself creating tons of component subclasses just to reuse some basic behavior in various contexts.

#### Code

```typescript
// The mediator interface declares a method used by components
// to notify the mediator about various events. The mediator may
// react to these events and pass the execution to other
// components.
interface Mediator is
    method notify(sender: Component, event: string)


// The concrete mediator class. The intertwined web of
// connections between individual components has been untangled
// and moved into the mediator.
class AuthenticationDialog implements Mediator is
    private field title: string
    private field loginOrRegisterChkBx: Checkbox
    private field loginUsername, loginPassword: Textbox
    private field registrationUsername, registrationPassword,
                  registrationEmail: Textbox
    private field okBtn, cancelBtn: Button

    constructor AuthenticationDialog() is
        // Create all component objects and pass the current
        // mediator into their constructors to establish links.

    // When something happens with a component, it notifies the
    // mediator. Upon receiving a notification, the mediator may
    // do something on its own or pass the request to another
    // component.
    method notify(sender, event) is
        if (sender == loginOrRegisterChkBx and event == "check")
            if (loginOrRegisterChkBx.checked)
                title = "Log in"
                // 1. Show login form components.
                // 2. Hide registration form components.
            else
                title = "Register"
                // 1. Show registration form components.
                // 2. Hide login form components

        if (sender == okBtn && event == "click")
            if (loginOrRegister.checked)
                // Try to find a user using login credentials.
                if (!found)
                    // Show an error message above the login
                    // field.
            else
                // 1. Create a user account using data from the
                // registration fields.
                // 2. Log that user in.
                // ...

// Components communicate with a mediator using the mediator
// interface. Thanks to that, you can use the same components in
// other contexts by linking them with different mediator
// objects.
class Component is
    field dialog: Mediator

    constructor Component(dialog) is
        this.dialog = dialog

    method click() is
        dialog.notify(this, "click")

    method keypress() is
        dialog.notify(this, "keypress")

// Concrete components don't talk to each other. They have only
// one communication channel, which is sending notifications to
// the mediator.
class Button extends Component is
    // ...

class Textbox extends Component is
    // ...

class Checkbox extends Component is
    method check() is
        dialog.notify(this, "check")
    // ...


// It is a behavioral pattern that allows a mediator (a central authority) to act as the coordinator between different objects, instead of the objects referring to each other directly.

class User {
	constructor(name, userId) {
		this.name = name;
		this.userId = userId;
		this.chatroom = null;
	}

	sendMessage(message, sendTo) {
		this.chatroom.send(message, this, sendTo);
	}

	recieveMessage(message, receiveFrom) {
		console.log(
			`${receiveFrom.name} sent the following message: ${message}`
		);
	}
}

class ChatRoom {
	constructor() {
		this.users = [];
	}

	register(user) {
		this.users[user.userId] = user;
		user.chatroom = this;
	}

	send(message, recieveFrom, sendTo) {
		sendTo.recieveMessage(message, recieveFrom);
	}
}

let mainChatRoom = new ChatRoom();
let peter = new User("Peter", 1);
let michael = new User("Michael", 2);

mainChatRoom.register(peter);
mainChatRoom.register(michael);
peter.sendMessage("Yo, Im Peter", michael);
michael.sendMessage("Hi Peter, Im Michael", peter);

```
