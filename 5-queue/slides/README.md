# 5 Queue

This is the opposite of a Stack. A Queue follows the

## FIFO Principle
> First in, first out.

This means that the items are returned in the same order in which they were added to the queue. The item that was added first is returned first and then the next one etc. The item that was added last is returned last. Like any Queue online or in front of the liquor store on New Year's Eve.

## Usage
If you have more work coming in than you can handle and want to make sure that they all get handled in fair oder. First come, first serve.
- e.g. Players queueing for Matchmaking, festival ticket booking or a full Server.

Or you just want to do the work later for some reason.
- e.g. the Game is paused right now.

If you want things to happen sequenced one after another.
- e.g. download a file, then load the file as a scene, then start the game.

If you want to queue up Game Events or Player Input
- e.g. multiple pop-ups appearing on Game Star, or a player queueing actions in a strategy game. Walk, then Attack the Tower, then Kill the goddamn sheep.

## Specification (C#)

Class: `TurboQueue<T>`

A Queue's common Interface contains the following:

```cs
// returns the current amount of items contained in the queue.
int Count;
// adds one item to the back of the queue.
void Enqueue(T item);
// returns the item in the front of the queue without removing it.
T Peek();
// returns the item in the front of the queue and removes it at the same time.
T Dequeue();
// removes all items from the queue.
void Clear();
// --------------- optional ---------------
// gets the iterator for this collection. Used by IEnumerable<T>-Interface to support foreach.
IEnumerator<T> IEnumerable<T>.GetEnumerator();
```

## Specification (C++)

Class: `template<typename T>TurboQueue`

A Queue's common Interface contains the following:

```cs
// returns true if there is no items in this queue
bool empty() const;
// returns the current amount of items contained in the queue.
size_t size() const;
// adds one item to the back of the queue.
void push(T item);
// returns the item in the front of the queue without removing it.
T& front();
const T& front() const;
// removes the item from the front of the queue.
void pop();
// --------------- optional ---------------
// gets the iterator for this collection.
Iterator<T> begin() const;
Iterator<T> end() const;
```

## Implementation
- Reference First Node
- Each Node references Next Node
- New Nodes get added after last Node
