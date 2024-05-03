# 201 Collections

## Introduction

Collections are used to store and group some variable number of data items that have some shared significance to the problem being solved and need to be operated upon together in some controlled fashion.

This module introduces you to a variety of collections that are used in several, most common contexts. As well as the `IEnumerable`-Interface, which allows easy iteration over your collection using `foreach`.

- [1 Collections](1-collections)
- [2 Iterator Pattern](2-iterator-pattern)
- [3 Linked Lists](3-linked-lists)
- [4 Stack](4-stack)
- [5 Queue](5-queue)
- [6 Complexity](6-complexity)
- [7 List (Dynamic Array)](7-list)
- [8 LINQ](8-linq)

## Passing Criteria
Implement either `TurboLinkedQueue` or `TurboLinkedList` including Unit Tests and the Demo Application.

## Excellent Criteria
On top of that, implement `TurboQueue` or `TurboList`, which internally uses an array for a buffer instead of a linked list.


## Learning Path

```mermaid
mindmap
  root((Algorithms and Data Structures))
    Data Structures
      C++: STL - Standard Template Library
      C#: System.Collections.Generic
      Iterator Pattern
        C#: IEnumerable, IEnumerator, yield
        C++: begin, end, iterator
      General Collections
        Internal Representation
          Linked Nodes
            Single-Linked
            Double-Linked
          Array
            Ring-Buffer
        Sequential
          List
          Queue
          Stack


```