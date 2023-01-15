# 6 Complexity
When handling collections, we often discuss an operation's complexity.
- We will learn the details in Part 2, Sorting.
- But in Short: The more elements in a collection we need to iterate over for a certain operation, the higher its complexity is.
  - O(1) means, that we can can always complete the operation without running over all nodes.
    - e.g. our `TurboLinkedQueue`'s `Dequeue` Function can directly dequeue the Node without running through all Nodes.
  - O(n) means, that we need to iterate over all nodes in order to complete an operation.
    - e.g. our `TurboLinkedQueue`'s `Count` Property needs to iterate over all nodes until it can say for sure, how many there are.
    - do you have an idea, how you could improve that?
    - implement it!

The solution to such problems is usually to cache some information. We often trade Complexity for Storage. In this case, the solutions are quite easy.

---

# 8 List

## Introduction

It will not require the `yield` Statement, no worries :)

Our class will be called `TurboList` and it will be a Generic Collection. Not that our implementation would be any better or faster than the original .NET implementation, but I want to avoid us having name conflicts with the existing `List<T>` all the time.

The Idea of a List is that it behaves very similar to an Array. But instead of giving a fixed size to the List, it should Resize itself automatically to our needs. In other words, we should be able to Add and Remove Items as we want. The Method `Add` just puts an item at the end of the List. Making it especially easy to add an item.

## Usage

Anything that you have a variable number of; e.g.:
- Items
- Players
- Enemies
- PowerUps
- Achievements
- Orders
- ...


## Specification

A List's common interface contains the following:

```cs
// returns the current amount of items contained in the list.
int Count;
// adds one item to the end of the list.
void Add(T item);
// gets the item at the specified index. If the index is outside the correct range, an exception is thrown.
T Get(int index);
// replaces the item at the specified index. If the index is outside the correct range, an exception is thrown.
void Set(int index, T value);
// removes all items from the list.
void Clear();
// removes one item from the list. If the 4th item is removed, then the 5th item becomes the 4th, the 6th becomes the 5th and so on.
void RemoveAt(int index);
// --------------- optional ---------------
// returns true, if the given item can be found in the list, else false.
bool Contains(T item);
// returns the index of the given item if it is in the list, else -1.
int IndexOf(T item);
// removes the first instance of the specified item from the list, if it can be found. Works similar to RemoveAt.
void Remove(T item);
// adds multiple items ad the end of this list at once. Works similar to Add.
void AddRange(IEnumerable<T> items);
// --------------- important, but difficult ---------------
// gets the iterator for this collection. Used by IEnumerable<T>-Interface to support foreach.
IEnumerator<T> IEnumerable<T>.GetEnumerator();
```

## Remarks

We have not discussed `Exceptions` in Detail, yet. But in its easiest way, you can throw an Exception like this:

```cs
throw new System.Exception("Describe your error well here, so the user understands why his application crashed.");
```

Your `TurboList` will need to be able to store a variable amount of data. It should do so by containing an Array as a field which sometimes will need to be resized. When resizing an Array, you need to create a new, larger Array that can fit all elements and then copy the elements from the old to the new Array.

```cs
private T[] items;
```

Hint: The Method `Array.Resize(ref Array array, int newSize)` exists. And yes, it would be the correct way to use it instead of manually resizing the Array. But that method needs to do exactly that, too (create a new array and copy all items) and I want to show you just how much work needs to be done when an Array is being resized. To make a point later on in this course :)

## Repository / Solution

For this implementation, we will set up a Solution which will allow you to work Test-Driven.

Here's a Guideline for how to use the DotNet CLI to do so: [CLI Multi-Project Setup](https://gist.github.com/InaSLew/a0b174f07fb657e8a3133daa3e942fb9)

## Application: CustomerManager

The Idea is to write an application that allows you to add and remove customers to a list.

```
Choose one option:
(1) Add a Customer
(2) Remove a Customer by name
(3) Remove a Customer by index
(4) Display all Customers
<<< 1
What is the Customer's name?
<<< Marc
Choose an option:
(1) Add a Customer
(2) Remove a Customer by name
(3) Remove a Customer by index
(4) Display all Customers
<<< 1
What is the Customer's name?
<<< Anna
Choose an option:
(1) Add a Customer
(2) Remove a Customer by name
(3) Remove a Customer by index
(4) Display all Customers
<<< 1
What is the Customer's name?
<<< Max
Choose an option:
(1) Add a Customer
(2) Remove a Customer by name
(3) Remove a Customer by index
(4) Display all Customers
<<< 4
0: Marc
1: Anna
2: Max
Choose an option:
(1) Add a Customer
(2) Remove a Customer by name
(3) Remove a Customer by index
(4) Display all Customers
<<< 2
What is the Customer's name?
<<< Marc
Choose an option:
(1) Add a Customer
(2) Remove a Customer by name
(3) Remove a Customer by index
(4) Display all Customers
<<< 4
0: Anna
1: Max
Choose an option:
(1) Add a Customer
(2) Remove a Customer by name
(3) Remove a Customer by index
(4) Display all Customers
<<< 3
What index?
<<< 1
Choose an option:
(1) Add a Customer
(2) Remove a Customer by name
(3) Remove a Customer by index
(4) Display all Customers
<<< 4
0: Anna
```

## You did it wrong!

Have you completed all of the steps for implementing the `TurboList`? Well, then chances are that you did it wrongly. Sorry. Well, not wrong really, but quite wasteful of resources.

Most probably, you resized the Array every time an element is added and probably also when it's removed? Like this:

| Action | List Count | Array Length | Garbage Created
:---------:|:------------:|:---------------:|:-------_:
|     |     0      |  0 | 0
| Add |     1      |  1 | 12
| Add |     2      |  2 | 16
| Add |     3      |  3 | 20
| Add |     4      |  4 | 24
| Remove |     3      |  3 | 28
| Add |     4      |  4 | 24
| Add |     5      |  5 | 28

What is the problem with this? Well, every time when the Array needs to be Resized, a new Array is created and the old one is dumped. This Creates Garbage.
- Garbage is the amount of bytes that need to be collected and freed by the Garbage Collector.
- Whenever a Heap Object is no longer referenced, the Garbage Collector needs to free its allocated memory address range, so it can be re-used for new objects.
- The Garbage collected includes:
  - 12 Bytes Overhead of the Array Class
  - 4 Bytes for each `int` Element

 That's a lot of wasted resources. Ideally, your list would do something like this:

| Action | List Count | Array Length | Garbage Created
:---------:|:------------:|:---------------:|:----------:
|     |     0      |  0 | 0
| Add |     1      |  4 | 12*
| Add |     2      |  4 | 0
| Add |     3      |  4 | 0
| Add |     4      |  4 | 0
| Remove |     3      |  4 | 0
| Add |     4      |  4 | 0
| Add |     5      |  8 | 28

*This Garbage could also be reduced to zero, if you use `Array.Empty<int>()` when initializing an empty array.
- The Method returns a shared static empty array.
- Instead of creating a new one.
- This is no problem, since an empty array can never change.

In other words: always make sure that there's some extra space in the buffer. It is quite common to always resize the array to double the size when needing to increase the size. And to never decrease it. This will reduce performance wasted on: Memory Allocation, Copying Memory and Garbage Collection. But it will instead waste resources on some unused Memory (if your list only needs 9 elements, then a buffer for 16 elements is quite wasteful).

This is very common with Algorithms and Data Structures: you either benefit in less Memory Consumption, or better CPU Performance.

---

# 9 LINQ

LINQ, or Language-Integrated-Query, is a technology by .NET which allows to handle Collections like Streams and manipulate them in a similar fashion to how you can manipulate Streams by nesting them. Imagine the following case:

```cs
Animal animals = zoo.GetAnimals();

class Cat : Animal {
    public string FavoriteFood;
    public FoodBowl FoodBowl;
}

class Animal {
    public bool IsHungry;
}

class FoodBowl{
    public void Fill(string food){}
}
```

Now, we notice that we have a lot of tuna left. We want to feed the cats, because they've not eaten in a while. But of course, we only want to feed the cats which are Hungry and like Tuna. Because cats are pretty picky, we have learned that the hard way. Also, they have their favorite FoodBowls. Cats are not easy to please.

We could implement it like this:

```cs
foreach(Animal animal in animals){
    if(animal.IsHungry){
        if(animal is Cat cat){
            if(cat.FavoriteFood == "Tuna"){
                cat.FoodBowl.Fill("Tuna");
            }
        }
    }
}
```

This should work. But what if it's not our Job to actually fill the bowl? We have an assistant that we need to provide with the information, so we should give him a list of all FoodBowls to fill with Tuna:

```cs
List<FoodBowl> foodBowls = new List<FoodBowl>();
foreach(Animal animal in animals){
    if(animal.IsHungry){
        if(animal is Cat cat){
            if(cat.FavoriteFood == "Tuna"){
                foodBowls.Add(cat.FoodBowl);
            }
        }
    }
}
assistant.OrderToFill("Tuna", foodBowls);
```

Neat! But also quite complex. Also, if we have thousands of cats, this will be a very large array again. What if we could Stream the Data to our Assistant? Fact is, we can!

```cs
IEnumerable<FoodBowl> foodBowls = animals
    .Where(animal => animal.IsHungry)
    .OfType<Cat>()
    .Where(cat => cat.FavoriteFood == "Tuna")
    .Select(cat => cat.FoodBowl);

assistant.OrderToFill("Tuna", foodBowls);
```

Pretty cool and expressive, or? And you can do so many more complex things with LINQ: 
- `Concat`: Combine to Collections
- `GroupBy`: Group a Collection by some common Property
- `First`: Get the first item. Exception if none.
- `FirstOrDefault`: Get the first item. Default if none.
- `Single`: Get one element for a condition. Exception if not exactly one.
- `Any`: Returns true, if any item satisfies a condition.
- `OrderBy`: Sort in ascending order.
- `OrderByDescending`: Sort in descending order.
- `ToDictionary`, `ToArray`, `ToList`: Convert to Collection-Type
- ... and many more.

## Usage:
- AI: Find all buildings of the player. Filter them by only the ones the AI has discovered. Filter them by the ones that seem not too well defended. Sort them by Priority. Attack the First.
- Multiplayer: Show Clan-Members. Filter by the ones being online OR being a High Rank. Sort them by Rank.


# Queue

We have learned about Lists and Stacks, the third of the most important Collections is the Queue. It is used, when things should get in order to be dealt with one at a time. They follow the

## FIFO Principle
> First in, first out.

This means that the items are returned in correct order. The one that was added first is returned first and then the next one etc. The item that was added last is returned last. Like any Queue online or in front of the liquor store New Year's Eve.

## Usage
If you have more work coming in than you can handle and want to make sure that they all get handled in fair oder. First come, first serve.
- e.g. Players queueing for Matchmaking, festival ticket booking or a full Server.

Or you just want to do the work later for some reason.
- e.g. the Game is paused right now.

If you want things to happen sequenced one after another.
- e.g. download a file, then load the file as a scene, then start the game.

If you want to queue up Game Events or Player Input
- e.g. multiple pop-ups appearing on Game Star, or a player queueing actions in a strategy game. Walk, then Attack the Tower, then Kill the goddamn sheep.

## Specification

Class: `TurboQueue<T>`

A Queue's common Interface contains the following:

```cs
// returns the current amount of items contained in the stack.
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

Well, this one is a bit more trouble-some than the Stack, isn't it?

The first, most obvious implementation is quite understandable. I recommend you to go for that one.

But then, look at the Dequeue-Method. It's a lot of work, isn't it? It's a bit like with the List:
- removing something from the end is super easy.
- but removing something from the front or in between? That requires a lot of shifting.

Try to think of a method using one Array that only needs ot move items around if the Buffer needs to be resized.

How would you go at that?

<details>
  <summary>Giving up? Check here.</summary>
  
- Check out Ring-Buffers.
</details>

## Application: Shunting-Yard-Algorithm

Implement the Shunting-Yard-Algorithm.\
Write a Console Application that asks you to enter any Mathematical Expression that's supposed to be resolved. The expressions may contain at least `+`, `-`, `*`, `/` and `()`.

Output:
```
Enter an expression:
<<< 12+(2+4)*3
Result: 30
```

---

# Further Research
- Read up on Linked Lists: Singly, Doubly, Circular
- Implement `TurboLinkedQueue` and `TurboLinkedStack` as Linked Lists.
- Can you think of other situations in which you could use the solution for the Queue-Spoiler?
- Try publish your TurboCollections on NuGet. The name `TurboCollections` is already taken, though :P
- Create a new Project outside of the solution. Add your NuGet-Package using `dotnet add package <packagename>`. Can you use your Library? How does updating the Library work?

---

# Additional Resources
- [TutorialsPoint](https://www.tutorialspoint.com/data_structures_algorithms/index.htm)
- [GeeksForGeeks](https://www.geeksforgeeks.org/data-structures/)
- [Programiz](https://www.programiz.com/dsa)