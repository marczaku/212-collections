# 7 List (Linked)

## 7.1 Tests
- Add a Test Project like in `4.1`
- Add Test Cases on your own

## 7.2 Class
Implement the following class:

```cs
public interface ITurboList<T> : IEnumerable<T> {
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
    // Also, we store the first instead of the last Node. First Come, First Serve.
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

## 5.3 Application: Spotify Song Queue
Write a `SpotifySongQueue` Console Application.
- Reference `TurboCollections`
- Use `TurboLinkedQueue`

The User repeatedly has the following options:
- `[s]kip to next song`: If there is another song in the queue, it prints `$"Now Playing {songName}."`. else, it says `"No more songs in the Queue. Add new Songs to the Queue first."`
- `[a]dd new song to the queue`: The application asks for a Song Name and then adds it to the Queue.

Sample Use:

```
Output:What would you like to do? [s]kip or [a]dd?
Input:a
Output:Enter the Song's Name
Input:ABBA - Mamma Mia
Output:What would you like to do? [s]kip or [a]dd?
Input:a
Output:Enter the Song's Name
Input:Shakira - Hips don't lie
Output:What would you like to do? [s]kip or [a]dd?
Input:s
Output:Now Playing: ABBA - Mamma Mia
Output:What would you like to do? [s]kip or [a]dd?
Input:s
Output:Now Playing: Shakira - Hips don't lie
Output:What would you like to do? [s]kip or [a]dd?
Input:s
Output:There is no more songs in the Queue.
Output:What would you like to do? [s]kip or [a]dd?
```