---
attachments: [factory.png]
tags: [Design Patterns]
title: Creational Patterns - Singleton
created: "2021-05-07T20:27:13.542Z"
modified: "2021-05-08T17:39:37.150Z"
---

// SINGLETON
// Intent:
// ensure that a given class had at most only one instance
// Design:
// text

# Creational Patterns - Singleton

###### Design patterns enable us to refactor code to provide clarity and intent through structure. They allow us to apply general solutions to common problems. Creational patterns are concerned with object creation mechanisms.

Singletons are classes has only one instance.

#### Objects

- **Singelton** This class doesn’t have a public constructor, so the only way to get its object is to call the getInstance method

<img src="https://refactoring.guru/images/patterns/diagrams/singleton/structure-en-2x.png" width="500" />

#### Application

- Use the Singleton pattern when a class in your program should have just a single instance available to all clients

#### Code

```typescript

// the singleton
class Database is
    static var instance: Database
    // The singleton's constructor should always be private to
    // prevent direct construction calls with the `new`
    // operator.
    private constructor Database() is
        // Some initialization code, such as the actual
        // connection to a database server.
        // ...
    // The static method  controls access to the singleton instance.
    public static function getInstance() is
        if (Database.instance == null)
              Database.instance = new Database()
        return Database.instance
    // shared logic
    public function query(sql) is


class Application is
    function main() is
        var foo: Database = Database.getInstance()
        foo.query("SELECT ...")
        // ...
        var foo: Database = Database.getInstance()
        bar.query("SELECT ...")
        // The variable `bar` will contain the same object as
        // the variable `foo`.
```
