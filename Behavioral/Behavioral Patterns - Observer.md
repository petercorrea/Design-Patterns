---
attachments: [factory.png]
tags: [Design Patterns]
title: Behavioral Patterns - Observer
created: "2021-05-08T17:51:25.774Z"
modified: "2021-05-08T18:07:55.096Z"
---

// OBSERVER
// Intent:
// allows objects to be notified of changes of another object
// Design:
// text

# Behavioral Patterns - Observer

###### Design patterns enable us to refactor code to provide clarity and intent through structure. They allow us to apply general solutions to common problems. Behavioral patterns are concerned with algorithms and the assignment of responsibilities between objects.

Observer is a behavioral design pattern that lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they’re observing.

#### Objects

- The **Publisher** issues events of interest to other objects. These events occur when the publisher changes its state or executes some behaviors. Publishers contain a subscription infrastructure that lets new subscribers join and current subscribers leave the list.

- When a new event happens, the publisher goes over the subscription list and calls the notification method declared in the subscriber interface on each subscriber object.

- The **Subscriber** interface declares the notification interface. In most cases, it consists of a single update method. The method may have several parameters that let the publisher pass some event details along with the update.

- **Concrete Subscribers** perform some actions in response to notifications issued by the publisher. All of these classes must implement the same interface so the publisher isn’t coupled to concrete classes.

- Usually, subscribers need some contextual information to handle the update correctly. For this reason, publishers often pass some context data as arguments of the notification method. The publisher can pass itself as an argument, letting subscriber fetch any required data directly.

- The **Client** creates publisher and subscriber objects separately and then registers subscribers for publisher updates.

<img src="https://refactoring.guru/images/patterns/diagrams/observer/structure-indexed.png" width="500" />

#### Application

- Use the Observer pattern when changes to the state of one object may require changing other objects, and the actual set of objects is unknown beforehand or changes dynamically.
- Use the pattern when some objects in your app must observe others, but only for a limited time or in specific cases.

#### Code

```typescript

// The base publisher class includes subscription management
// code and notification methods.
class EventManager is
    private field listeners: hash map of event types and listeners

    method subscribe(eventType, listener) is
        listeners.add(eventType, listener)

    method unsubscribe(eventType, listener) is
        listeners.remove(eventType, listener)

    method notify(eventType, data) is
        foreach (listener in listeners.of(eventType)) do
            listener.update(data)

// The concrete publisher contains real business logic that's
// interesting for some subscribers. We could derive this class
// from the base publisher, but that isn't always possible in
// real life because the concrete publisher might already be a
// subclass. In this case, you can patch the subscription logic
// in with composition, as we did here.
class Editor is
    public field events: EventManager
    private field file: File

    constructor Editor() is
        events = new EventManager()

    // Methods of business logic can notify subscribers about
    // changes.
    method openFile(path) is
        this.file = new File(path)
        events.notify("open", file.name)

    method saveFile() is
        file.write()
        events.notify("save", file.name)

    // ...

// Here's the subscriber interface. If your programming language
// supports functional types, you can replace the whole
// subscriber hierarchy with a set of functions.
interface EventListener is
    method update(filename)

// Concrete subscribers react to updates issued by the publisher
// they are attached to.
class LoggingListener implements EventListener is
    private field log: File
    private field message

    constructor LoggingListener(log_filename, message) is
        this.log = new File(log_filename)
        this.message = message

    method update(filename) is
        log.write(replace('%s',filename,message))

class EmailAlertsListener implements EventListener is
    private field email: string

    constructor EmailAlertsListener(email, message) is
        this.email = email
        this.message = message

    method update(filename) is
        system.email(email, replace('%s',filename,message))


// An application can configure publishers and subscribers at
// runtime.
class Application is
    method config() is
        editor = new Editor()

        logger = new LoggingListener(
            "/path/to/log.txt",
            "Someone has opened the file: %s")
        editor.events.subscribe("open", logger)

        emailAlerts = new EmailAlertsListener(
            "admin@example.com",
            "Someone has changed the file: %s")
        editor.events.subscribe("save", emailAlerts)

        // It allows objects (observers) that have subscribed to an event to wait for input and react to it when notified. This means they don’t have to continuously keep checking whether the input has been provided or not.

class Subject {
	constructor() {
		this.observersList = [];
		this.newArticlePosted = false;
		this.articleName = null;
	}

	subscribe(observer) {
		this.observersList.push(observer);
	}

	unsubscribe(observer) {
		this.observersList = this.observersList.filter(
			(obj) => obj !== observer
		);
	}

	notify() {
		if (this.newArticlePosted) {
			this.observersList.forEach((subscriber) => subscriber.update());
		} else {
			return;
		}
	}

	getUpdate() {
		return this.articleName;
	}

	postNewArticle(articleName) {
		this.articleName = articleName;
		this.newArticlePosted = true;
		this.notify();
	}
}

class Observer {
	constructor() {
		this.subject = new Subject();
	}

	update() {
		if (subject.getUpdate() == null) {
			console.log("No new article");
		} else {
			console.log(`The new article ${subject.getUpdate()} is posted`);
		}
	}

	setSubject(subject) {
		this.subject = subject;
	}
}

var subject = new Subject();
var observer = new Observer();
observer.setSubject(subject);
subject.subscribe(observer);
observer.update();
subject.postNewArticle("Dark matter");

```
