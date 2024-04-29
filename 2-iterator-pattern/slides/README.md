# 2 Iterator Pattern

The Iterator Pattern is a Design Patterns used in Programming to allow us to sequentially visit all items in any type of collection.

## Design Patterns

Design Patterns are known, battle-proven solutions to problems commonly encountered in programming. Usually, you can't solve those categories of problems once and for all because of little differences and nuances in the details of the problem, but the correct Design Pattern an give you a great set of instructions on how to approach and implement the problem for your use case.

We will learn a lot more Design Patterns at a later point :)

## The Problem

We will learn about many different kinds of Collections. They all store items in different ways. Many of which don't have a clear order or a clear beginning and an end.

But then, how can we define a standardized way for looking at all items in a collection?

## The Solution (Pattern)

```cs
public interface ICollection {
    // Returns a class used for iterating over all elements
    IIterator GetIterator();
}
```

```cs
public interface IIterator {
    // tells you, whether there is any more elements
    bool HasNext();
    // returns the next element
    object GetNext();
}
```

Now, this Interface can be used as follows:

```cs
ICollection anyCollection = CreateRandomCollection();
IIterator iterator = anyCollection.GetIterator();
while(iterator.HasNext()){
    Console.WriteLine($"Next Element: {iterator.GetNext()}");
}
```

## The Solution (C#)

### IEnumerable

The `IEnumerable`-Interface is defined twice. Once as `System.Collections.IEnumerable` and once as `System.Collections.Generic.IEnumerable<T>`. I guess you know, why? Because the latter is Type-Safe and used for Generic Collections.

The Purpose is for a class to implement the Iterator-Pattern, which is a very popular Design Pattern. A Design Pattern is a Pattern that has been specified as a solution to a certain kind of problem. Usually, Design Patterns require you to implement them for your specific use-case, but the code-structure usually looks very similar to the original.

The Iterator-Pattern is a smart solution which allows you to easily iterate over a collection of variable elements.

To iterate means to go through it and look at all elements which are stored in the Collection.

You need to know, that not all collections have an Order to it. Which means, that not all Collections allow you to use the indexer like `collection[0]` and `collection[25]` to access the elements at a certain position in the collection.

Iterators solve the problem, because you get access to all elements without any guaranteed order.

The Interface is defined as such:

```cs
namespace System.Collections;

public interface IEnumerable
{
    // Returns an IEnumerator for this enumerable Object.  The enumerator provides
    // a simple way to access all the contents of a collection.
    IEnumerator GetEnumerator();
}
```

Alright. This does not look all too interesting. All the Magic happens in the `IEnumerator`-Interface, I suppose?

### IEnumerator

```cs
namespace System.Collections;

public interface IEnumerator
{
    // Advances the enumerator to the next element of the enumeration and
    // returns a boolean indicating whether an element is available. Upon
    // creation, an enumerator is conceptually positioned before the first
    // element of the enumeration, and the first call to MoveNext
    // brings the first element of the enumeration into view.
    //
    bool MoveNext();

    // Returns the current element of the enumeration. The returned value is
    // undefined before the first call to MoveNext and following a
    // call to MoveNext that returned false. Multiple calls to
    // GetCurrent with no intervening calls to MoveNext
    // will return the same object.
    //
    object? Current
    {
        get;
    }

    // Resets the enumerator to the beginning of the enumeration, starting over.
    // The preferred behavior for Reset is to return the exact same enumeration.
    // This means if you modify the underlying collection then call Reset, your
    // IEnumerator will be invalid, just as it would have been if you had called
    // MoveNext or Current.
    //
    void Reset();
}
```

Alright, the Interface looks straightforward enough, doesn't it?

`bool MoveNext()` moves the Iterator to the next Element in the Collection. In the beginning, it is placed BEFORE the First Element. Which means, that you need to move it first before trying to access any element. It returns `true`, if it successfully moved the Iterator. If it returns `false`, it means that you've reached over the end of the Collection.

`Current {get;}` gives you the current Element of the Collection. It points at different Elements after calling `MoveNext`.

`void Reset()` allows you to reset the Enumerator, in case you want to start over from the first element again. This is pretty uncommon.

### foreach

If you have done the previous Exercise, then you have actually implemented your own Implementation of `foreach`.

`foreach` is only syntactic sugar which makes Code especially readable, if you want to iterate over a collection. Instead of calling `GetEnumerator()`, `MoveNext()` and `Current`, you can simply write:

```cs
using System.Collections.Generic;

List<int> numbers = new List<int>();
numbers.Add(2);
numbers.Add(5);
foreach(int number in numbers){
    System.Console.WriteLine(number);
}
```

Pretty neat, or? Now, you've come to understand that this keyword does not do any Magic, but just uses existing Program Code under the hood. And that you are able to provide `foreach`-Support for your class by implementing the `IEnumerable` or `IEnumerable<T>`-Interface.

## The Solution (C++)

### begin() and end()

- `begin()` returns an `Iterator` that points to the first element of the collection.
- `end()` returns an `Iterator` that points behind the last element of the collection.

```c++
class IterableCollection<T>
{
    Iterator<T> begin() const;
    Iterator<T> end() const;
};
```

### Iterator
- `++` moves the iterator to the next element
- `*` the dereferencing operator returns the value that the iterator currently points to
- `==` returns true if 2 operators point to the same element
- `!=` returns true if 2 operators point to different elements

```c++
class Iterator<T>{
    Iterator<T>& operator++();
    T& operator*() const;

    // Equality operators
    bool operator==(const Iterator<T>& other) const;
    bool operator!=(const Iterator<T>& other) const;
};
```

### foreach

Similarly to C#, C++' `foreach` keyword uses the iterator that can be received using `begin()` to retrieve each element of a collection and is much easier to use than iterating manually:

```c++
#include <vector>
#include <iostream>

using namespaces td;

vector<int> numbers{};
numbers.push_back(2);
numbers.push_back(5);
foreach(int& number : numbers){
    cout << number << "\n";
}
```

## yield (C# Only)

### Introduction

Now, `yield` is one of the keywords that struck me as Black Magic when I saw it the first time. I think that it is best explained through a code sample:

```cs
using System.Collections;

static IEnumerable GetNumbersUntil(int number) {
    for(int i = 0; i < number; i++) {
        yield return i;
    }
}

foreach(var number in GetNumbersUntil(5)) {
    System.Console.WriteLine(number);
}
```

Output:
```
0
1
2
3
4
```

So, what has happened here? Somehow, the method was able to return multiple numbers. Does it pause and then continue again, or does it return a Collection of all numbers? Let's find out:

### Step-By-Step

```cs
using System.Collections;
using System;

static IEnumerable GetNumbersUntil(int number) {
    Console.WriteLine("[GetNumbersUntil] Start");
    for(int i = 0; i < number; i++) {
        Console.WriteLine($"[GetNumbersUntil] Before yield return {i}");
        yield return i;
        Console.WriteLine($"[GetNumbersUntil] After yield return {i}");
    }
    Console.WriteLine($"[GetNumbersUntil] Finish");
}

foreach(var number in GetNumbersUntil(2))
{
    Console.WriteLine($"[Main] Got Number: {number}");
}
```

Output:
```
[GetNumbersUntil] Start
[GetNumbersUntil] Before yield return 0
[Main] Got Number: 0
[GetNumbersUntil] After yield return 0
[GetNumbersUntil] Before yield return 1
[Main] Got Number: 1
[GetNumbersUntil] After yield return 1
[GetNumbersUntil] Finish
```

We can see, that the Method gets interrupted by the `yield` Statement, to return a value. And then can be returned to, when the `foreach` statement is triggered again.

We already know that `foreach` calls `GetEnumerator` first. This returns an Iterator-Object for this method.

Then, `foreach` calls `MoveNext()` in every iteration, which makes the Iterator execute code from the object until the next `yield return` statement, making the returned value available as `Current` and returning `true`, so the `foreach` continues.

If `MoveNext()` hits a `yield break` or the end of the method, then it returns `false` and the `foreach` Loop ends.

### Under the Hood

But how is it possible, that a Method gets interrupted? We can take a look at the [Compiled C# Code](https://sharplab.io/#v2:CYLg1APgAgDABFAjAOgMIHsA2mCmBjAFwEt0A7AZwG4BYAKCgCZE66kA2BRAZjgHEcCAOQCuAWwBGOAE7kAqqWKYAFEQVxSYyVICUcAN504RzgE4lAIgDa/IZulyFRTAF04AZQIBDKQXPaatMZwAGboUipqRHAAvHAwlHBRADzqdlIJRGBgugaBQcZIZgAkVjYiEvbyiq4AQjihUjiciAgA7PpEAL5+AfkFiC1Q7US9faZKJdYC5VoO1XAAgsEE0s1tHd3+hsad20aFE6XTaXNOrgBiqkTkABY9dLu0dA04nng3SgBu3qkVUomkPjHP6nZQMbTaOi5IIHSYAWU8qlcvHQBDgM2kIH0Gj+mwCnSAA).

Here, you'll see the following (simplified):

```cs
private sealed class GetNumbersUntil : IEnumerable<object>, IEnumerable, IEnumerator<object>, IDisposable, IEnumerator
{
    private int __state;

    private object __current;

    private int number;

    private int i;

    object IEnumerator<object>.Current
    {
        [DebuggerHidden]
        get
        {
            return __current;
        }
    }

    [DebuggerHidden]
    public GetNumbersUntil(int __state)
    {
        this.__state = __state;
    }

    private bool MoveNext()
    {
        int num = __state;
        if (num != 0)
        {
            if (num != 1)
            {
                return false;
            }
            __state = -1;
            Console.WriteLine(string.Format("[GetNumbersUntil] After yield return {0}", i));
            i++;
        }
        else
        {
            __state = -1;
            Console.WriteLine("[GetNumbersUntil] Start");
            i = 0;
        }
        if (i < number)
        {
            Console.WriteLine(string.Format("[GetNumbersUntil] Before yield return {0}", i));
            __current = i;
            __state = 1;
            return true;
        }
        Console.WriteLine("[GetNumbersUntil] Finish");
        return false;
    }

    [DebuggerHidden]
    IEnumerator<object> IEnumerable<object>.GetEnumerator()
    {
        if (thisStateMachineIsReset)
        {
            return this;
        }
        else
        {
            return new StateMachine();
        }
    }
}
```

Woah, that's not easy to read. But basically, our Method gets compiled into a `class` which is a State Machine (Note: We'll learn about State Machines later, in Design Pattern classes). That increases `__state` every time `MoveNext()` is called and thereby moves on to the next statement. Also, `GetEnumerator` returns the State Machine itself as an `Enumerable`, but only, if it's currently not in use. Else, it will return a new State Machine.

Again, not really Black Magic, our Computer does not need to learn to interrupt methods. Instead, the Compiler converts our simple-looking code into something quite complex to enable this to work.

### Why not simply return a List?

For the previous example, we also could've written the following code:

```cs
using System.Collections;

static IEnumerable GetNumbersUntil(int number) {
    int[] result = new int[number];
    for(int i = 0; i < number; i++) {
        result[i] = i;
    }
    return result;
}
```

But what's the disadvantage here?

You'll find out, if you try executing the following:

```cs
double total = 0;
foreach(int number in GetNumbersUntil(1_000_000_000)){
    total += number;
}
Console.WriteLine($"Total: {total}");
```

Output:
```
Total: 4.99999999067109E+17
```

This should work just fine (though take a while), if you used `yield`, but probably it will take forever or crash, if you use the solution that returns an Array. 

### Question
Why?

### One at a time

Because `yield` allows you to use a technique called `Streaming`. Where instead of doing all work at once and returning a giant object containing the whole result, you do the work bit by bit and always return a small bit of the result. This makes apps more responsive and can reduce Memory Usage in this case from 4GB to to 4 bytes!

The same technique is used almost everywhere else in Programming. For example, when you stream a show on Netflix. Instead of downloading the whole 4GB Movie, it will stream it bit by bit. Allowing you to start watching after a few kiloBytes already.

In other words: Don't underestimate the power of `yield` and use it where ever it makes sense.

### Coroutines

Let's have a look at Unity's Coroutines again:

```cs
using UnityEngine;
using System.Collections;

IEnumerator Despawn(){
    Debug.Log("Hello there!");
    yield return new WaitForSeconds(2);
    Debug.Log("Hello from the future!");
}
StartCoroutine(Despawn());
```

So, what Unity does also is not any sort of Black Magic. When you call `StartCoroutine` and pass it an `IEnumerator`, it will continue calling `MoveNext` until it returns false. If `Current` points to a `WaitForSeconds`-Object, then Unity will know to wait until the two seconds are over before calling `MoveNext` again. Brilliant :)

---