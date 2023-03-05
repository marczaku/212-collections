# 7 List (Linked)

## 7.1 Tests
- Add a Test Project like in `4.1`
- Add Test Cases on your own

## 7.2 Class
Implement the following class:

```cs
public interface ITurboList<T> : IEnumerable<T> {
  // returns the current amount of items contained in the list.
  int Count {get;}
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
  // IEnumerator<T> IEnumerable<T>.GetEnumerator();
}
```

```cs
public class TurboLinkedList<T> : ITurboList<T> {
    // This class is VERY similar to the TurboLinkedStack
    class Node {
        public T Value;
        // But we store the Next Node for each Node instead.
        public Node Next;
    }
    // Since we iterate over a List in the correct order, we should keep a reference to the First Node:
    Node FirstNode;

    public void Add(T value){
        // Check out Enqueue in 5.2 TurboLinkedQueue
    }

    T Get(int index){
      // run through the Nodes Node by Node until you reach the correct index
      // then return the value of that node
    }

    // ...

    // Everything else is super similar to the TurboLinkedStack!
}
```

## 7.3 Application: CustomerManager

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