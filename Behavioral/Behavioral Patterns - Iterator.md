---
attachments: [factory.png]
tags: [Design Patterns]
title: Behavioral Patterns - Iterator
created: "2021-05-08T17:50:42.854Z"
modified: "2021-05-08T17:59:29.541Z"
---

// ITERATOR
// Intent:
// traverse elements of a collection without worrying about the underlying representation
// Design:
//

# Behavioral Patterns - Iterator

###### Design patterns enable us to refactor code to provide clarity and intent through structure. They allow us to apply general solutions to common problems. Behavioral patterns are concerned with algorithms and the assignment of responsibilities between objects.

Iterator is a behavioral design pattern that lets you traverse elements of a collection without exposing its underlying representation (list, stack, tree, etc.).

#### Objects

- The **Iterator** interface declares the operations required for traversing a collection: fetching the next element, retrieving the current position, restarting iteration, etc.

- **Concrete Iterators** implement specific algorithms for traversing a collection. The iterator object should track the traversal progress on its own. This allows several iterators to traverse the same collection independently of each other.

- The **Collection** interface declares one or multiple methods for getting iterators compatible with the collection. Note that the return type of the methods must be declared as the iterator interface so that the concrete collections can return various kinds of iterators.

- **Concrete Collections** return new instances of a particular concrete iterator class each time the client requests one. You might be wondering, where’s the rest of the collection’s code? Don’t worry, it should be in the same class. It’s just that these details aren’t crucial to the actual pattern, so we’re omitting them.

- The **Client** works with both collections and iterators via their interfaces. This way the client isn’t coupled to concrete classes, allowing you to use various collections and iterators with the same client code.

Typically, clients don’t create iterators on their own, but instead get them from collections. Yet, in certain cases, the client can create one directly; for example, when the client defines its own special iterator.

<img src="https://refactoring.guru/images/patterns/diagrams/iterator/structure-indexed.png" width="500" />

#### Application

- Use the Iterator pattern when your collection has a complex data structure under the hood, but you want to hide its complexity from clients (either for convenience or security reasons).
- Use the pattern to reduce duplication of the traversal code across your app.
- Use the Iterator when you want your code to be able to traverse different data structures or when types of these structures are unknown beforehand.

#### Code

```typescript
// The collection interface must declare a factory method for
// producing iterators. You can declare several methods if there
// are different kinds of iteration available in your program.
interface SocialNetwork is
    method createFriendsIterator(profileId):ProfileIterator
    method createCoworkersIterator(profileId):ProfileIterator


// Each concrete collection is coupled to a set of concrete
// iterator classes it returns. But the client isn't, since the
// signature of these methods returns iterator interfaces.
class Facebook implements SocialNetwork is
    // ... The bulk of the collection's code should go here ...

    // Iterator creation code.
    method createFriendsIterator(profileId) is
        return new FacebookIterator(this, profileId, "friends")
    method createCoworkersIterator(profileId) is
        return new FacebookIterator(this, profileId, "coworkers")


// The common interface for all iterators.
interface ProfileIterator is
    method getNext():Profile
    method hasMore():bool


// The concrete iterator class.
class FacebookIterator implements ProfileIterator is
    // The iterator needs a reference to the collection that it
    // traverses.
    private field facebook: Facebook
    private field profileId, type: string

    // An iterator object traverses the collection independently
    // from other iterators. Therefore it has to store the
    // iteration state.
    private field currentPosition
    private field cache: array of Profile

    constructor FacebookIterator(facebook, profileId, type) is
        this.facebook = facebook
        this.profileId = profileId
        this.type = type

    private method lazyInit() is
        if (cache == null)
            cache = facebook.socialGraphRequest(profileId, type)

    // Each concrete iterator class has its own implementation
    // of the common iterator interface.
    method getNext() is
        if (hasMore())
            currentPosition++
            return cache[currentPosition]

    method hasMore() is
        lazyInit()
        return currentPosition < cache.length


// Here is another useful trick: you can pass an iterator to a
// client class instead of giving it access to a whole
// collection. This way, you don't expose the collection to the
// client.
//
// And there's another benefit: you can change the way the
// client works with the collection at runtime by passing it a
// different iterator. This is possible because the client code
// isn't coupled to concrete iterator classes.
class SocialSpammer is
    method send(iterator: ProfileIterator, message: string) is
        while (iterator.hasMore())
            profile = iterator.getNext()
            System.sendEmail(profile.getEmail(), message)


// The application class configures collections and iterators
// and then passes them to the client code.
class Application is
    field network: SocialNetwork
    field spammer: SocialSpammer

    method config() is
        if working with Facebook
            this.network = new Facebook()
        if working with LinkedIn
            this.network = new LinkedIn()
        this.spammer = new SocialSpammer()

    method sendSpamToFriends(profile) is
        iterator = network.createFriendsIterator(profile.getId())
        spammer.send(iterator, "Very important message")

    method sendSpamToCoworkers(profile) is
        iterator = network.createCoworkersIterator(profile.getId())
        spammer.send(iterator, "Very important message")


        // The iterator pattern allows the definition of various types of iterators that can be used to iterate a collection of objects sequentially without exposing the underlying form.
class Iterator {
	constructor(collection) {
		this.index = 0;
		this.collection = collection;
	}

	next() {
		return this.collection[this.index++];
	}

	hasNextElement() {
		// Starts at idx 1
		return this.index <= this.collection.length;
	}

	first() {
		this.index = 0;
		return this.next();
	}
}

function iterate() {
	let collection = ["Yellow", "Blue", "Green"];

	let iter = new Iterator(collection);

	for (let i = iter.first(); iter.hasNextElement(); i = iter.next()) {
		console.log(i);
	}
}

iterate();

// Another Example
class ReverseIterator {
	//define-your-reverse-iterator-here
	constructor(collection) {
		this.collection = collection;
		this.keys = Object.keys(collection);
		this.index = this.keys.length - 1;
	}

	hasprevElement() {
		return this.index >= 0;
	}

	last() {
		this.index = this.keys.length - 1;
		return this.collection[this.keys[this.index]];
	}

	previous() {
		return this.collection[this.keys[--this.index]];
	}
}

function reverseIterate(items) {
	//write-your-code-here
	//to display the values of keys
	//in items in reverse
	let iter = new ReverseIterator(items);

	for (let i = iter.last(); iter.hasprevElement(); i = iter.previous()) {
		console.log(i);
	}
}

reverseIterate({
	name: "Anne",
	age: "23",
	gender: "Female",
	Occupation: "Engineer",
});


```
