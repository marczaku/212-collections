# 7 List

## Introduction

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

### Exceptions / Error Handling

A reminder regarding Exceptions. You can throw Exceptions like this:

```cs
// C#
throw new System.Exception("Describe your error well here, so the user understands why his application crashed.");
```

```c++
// C++
throw "Describe your error well here, so the user understands why his application crashed.";
```

### Resizing

Your `TurboList` will need to be able to store a variable amount of data. It should do so by containing an Array as a field which sometimes will need to be resized. When resizing an Array, you need to create a new, larger Array that can fit all elements and then copy the elements from the old to the new Array.

```cs
private T[] items;
```

Hint: The Method `Array.Resize(ref Array array, int newSize)` exists. And yes, it would be the correct way to use it instead of manually resizing the Array. But that method needs to do exactly that, too (create a new array and copy all items) and I want to show you just how much work needs to be done when an Array is being resized. To make a point later on in this course :)

## Repository / Solution

For this implementation, we will set up a Solution which will allow you to work Test-Driven.


<details>
  <summary> Open after finishing the implementation using an Array.</summary>

Have you completed all of the steps for implementing the `TurboList`? Well, then chances are that you did it wrongly. Sorry. Well, not wrong really, but quite wasteful of resources.

Most probably, you resized the Array every time an element is added and probably also when it's removed? Like this:

| Action | List Count | Array Length | Garbage Created |
|:------:|:----------:|:------------:|:---------------:|
|        |     0      |      0       |        0        |
|  Add   |     1      |      1       |       12        |
|  Add   |     2      |      2       |       16        |
|  Add   |     3      |      3       |       20        |
|  Add   |     4      |      4       |       24        |
| Remove |     3      |      3       |       28        |
|  Add   |     4      |      4       |       24        |
|  Add   |     5      |      5       |       28        |

What is the problem with this? Well, every time when the Array needs to be Resized, a new Array is created and the old one is dumped. This Creates Garbage.
- Garbage is the amount of bytes that need to be collected and freed by the Garbage Collector.
- Whenever a Heap Object is no longer referenced, the Garbage Collector needs to free its allocated memory address range, so it can be re-used for new objects.
- The Garbage collected includes:
  - 12 Bytes Overhead of the Array Class
  - 4 Bytes for each `int` Element

 That's a lot of wasted resources. Ideally, your list would do something like this:

| Action | List Count | Array Length | Garbage Created |
|:------:|:----------:|:------------:|:---------------:|
|        |     0      |      0       |        0        |
|  Add   |     1      |      4       |       12*       |
|  Add   |     2      |      4       |        0        |
|  Add   |     3      |      4       |        0        |
|  Add   |     4      |      4       |        0        |
| Remove |     3      |      4       |        0        |
|  Add   |     4      |      4       |        0        |
|  Add   |     5      |      8       |       28        |

*This Garbage could also be reduced to zero, if you use `Array.Empty<int>()` when initializing an empty array.
- The Method returns a shared static empty array.
- Instead of creating a new one.
- This is no problem, since an empty array can never change.

In other words: always make sure that there's some extra space in the buffer. It is quite common to always resize the array to double the size when needing to increase the size. And to never decrease it. This will reduce performance wasted on: Memory Allocation, Copying Memory and Garbage Collection. But it will instead waste resources on some unused Memory (if your list only needs 9 elements, then a buffer for 16 elements is quite wasteful).

This is very common with Algorithms and Data Structures: you either benefit in less Memory Consumption, or better CPU Performance.

</details>