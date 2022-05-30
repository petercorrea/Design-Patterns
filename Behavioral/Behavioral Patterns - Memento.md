---
attachments: [factory.png]
tags: [Design Patterns]
title: Behavioral Patterns - Memento
created: "2021-05-08T17:51:14.062Z"
modified: "2021-05-08T18:05:54.539Z"
---

// MEMETO
// Intent:
// save and restore the previous state of an object without revealing the details of the implementation
// Design:
// text

# Behavioral Patterns - Memento

###### Design patterns enable us to refactor code to provide clarity and intent through structure. They allow us to apply general solutions to common problems. Behavioral patterns are concerned with algorithms and the assignment of responsibilities between objects.

Memento is a behavioral design pattern that lets you save and restore the previous state of an object without revealing the details of its implementation.

#### Objects

- The **Originator** class can produce snapshots of its own state, as well as restore its state from snapshots when needed.

- The **Memento** is a value object that acts as a snapshot of the originator’s state. It’s a common practice to make the memento immutable and pass it the data only once, via the constructor.

- The **Caretaker** knows not only “when” and “why” to capture the originator’s state, but also when the state should be restored.

A caretaker can keep track of the originator’s history by storing a stack of mementos. When the originator has to travel back in history, the caretaker fetches the topmost memento from the stack and passes it to the originator’s restoration method.

- In this implementation, the memento class is nested inside the **originator**. This lets the originator access the fields and methods of the memento, even though they’re declared private. On the other hand, the caretaker has very limited access to the memento’s fields and methods, which lets it store mementos in a stack but not tamper with their state.

There are several ways to implement this. For more options look at https://refactoring.guru/design-patterns/memento

<img src="https://refactoring.guru/images/patterns/diagrams/memento/structure1-indexed.png" width="500" />

#### Application

- Use the Memento pattern when you want to produce snapshots of the object’s state to be able to restore a previous state of the object.
- Use the pattern when direct access to the object’s fields/getters/setters violates its encapsulation.
-

#### Code

```typescript

// The originator holds some important data that may change over
// time. It also defines a method for saving its state inside a
// memento and another method for restoring the state from it.
class Editor is
    private field text, curX, curY, selectionWidth

    method setText(text) is
        this.text = text

    method setCursor(x, y) is
        this.curX = curX
        this.curY = curY

    method setSelectionWidth(width) is
        this.selectionWidth = width

    // Saves the current state inside a memento.
    method createSnapshot():Snapshot is
        // Memento is an immutable object; that's why the
        // originator passes its state to the memento's
        // constructor parameters.
        return new Snapshot(this, text, curX, curY, selectionWidth)

// The memento class stores the past state of the editor.
class Snapshot is
    private field editor: Editor
    private field text, curX, curY, selectionWidth

    constructor Snapshot(editor, text, curX, curY, selectionWidth) is
        this.editor = editor
        this.text = text
        this.curX = curX
        this.curY = curY
        this.selectionWidth = selectionWidth

    // At some point, a previous state of the editor can be
    // restored using a memento object.
    method restore() is
        editor.setText(text)
        editor.setCursor(curX, curY)
        editor.setSelectionWidth(selectionWidth)

// A command object can act as a caretaker. In that case, the
// command gets a memento just before it changes the
// originator's state. When undo is requested, it restores the
// originator's state from a memento.
class Command is
    private field backup: Snapshot

    method makeBackup() is
        backup = editor.createSnapshot()

    method undo() is
        if (backup != null)
            backup.restore()
    // ...
```
