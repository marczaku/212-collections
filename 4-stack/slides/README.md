# 4 Stack (Linked)

Let's implement our first own Collection! The Stack is used whenever you want to have a collection where the latest element is returned first. Imagine it like a Stack of Plates. You usually put Plates on the top of a stack and you also take plates from the top of the stack first.

## LIFO Principle
> Last in, first out.

This means that the items are returned in reverse order. The one that was added last is returned first and then the previous one etc. The item that was added first is returned last.

## Usage
If you have a bunch of work and need to:
- pause some work when other comes in.
- then continue when the new work is done.
- very often used in algorithms like Pathfinding, AI, ...
- also, every computer program (Call Stack)

If you have a game with a stack of things, where only the top one can be retrieved
- e.g. a deck of cards

Game States and UI States
- e.g. Main Menu (bottom) -> Settings -> Confirm Popup (top)

History
- e.g. Browser History, File Explorer History, Undo/Redo

## Specification (C#)

Class: `TurboStack<T>`

A Stack's common Interface contains the following:

```cs
// returns the current number of items contained in the stack.
int Count;
// adds one item on top of the stack.
void Push(T item);
// returns the item on top of the stack without removing it.
T Peek();
// returns the item on top of the stack and removes it at the same time.
T Pop();
// removes all items from the stack.
void Clear();
// --------------- BONUS ---------------
// gets the iterator for this collection. Used by IEnumerable<T>-Interface to support foreach.
IEnumerator<T> IEnumerable<T>.GetEnumerator();
```

## Specification (C++)

Class: `<template <typename T>TurboStack`

A Stack's common Interface contains the following:

```c++
// returns true if there is no item on the stack.
bool empty() const;
// returns the current number of items contained in the stack.
size_t size() const;
// adds one item on top of the stack.
void push(const T& item);
// returns the item on top of the stack without removing it.
T& top();
const T& top() const;
// removes the top item from th stack.
void pop()
// --------------- BONUS ---------------
// gets the iterator for this collection.
Iterator<T> begin() const;
Iterator<T> end() const;
```

### Application: GameStateHistory

Write a `GameStateHistory` - Console Application in which multiple states exist. 
- From Main Menu, you can go to Level 1, Settings or Quit.
- From Settings you can not go anywhere
- On Quit the Application Quits
- From Level 1, you can go to Level 2 or the Main Menu.
- From Level 2, you can go to Level 3 or the Main Menu
- etc.

On top of that, you have one more options at all times (unless it's not possible):
- Go Backward: Goes back to the previous State in the history

It should look something like this:

```
You are here: Main Menu
What do you want to do?
(0): Go to Level 1
(1): Go to Settings
<<< 0
You are here: Level 1
What do you want to do?
(0): Go to Level 2
(1): Go to Main Menu
(b): Go back to Main Menu
<<< 0
You are here: Level 2
What do you want to do?
(0): Go to Level 3
(1): Go to Main Menu
(b): Go back to Level 1
<<< b
You are here: Level 1
What do you want to do?
(0): Go to Level 2
(1): Go to Main Menu
(b): Go back to Main Menu
```

Use a Stack to keep track of the History.

- What if you also wanted to be able to go forward again?

---