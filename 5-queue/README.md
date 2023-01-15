# 5 Queue (Linked)

## 5.1 Tests
- Add a Test Project like in `4.1`
- Add Test Cases on your own

## 5.2 Class
Implement the following class:

```cs
public interface ITurboQueue<T> : IEnumerable<T> {
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
}
```

```cs
public class TurboLinkedQueue<T> : ITurboQueue<T> {
    // This class is VERY similar to the TurboLinkedStack
    class Node {
        public T Value;
        // But we store the Next Node for each Node instead.
        public Node Next;
    }
    // Also, we store the first instead of the last Node. First Come, First Serve.
    Node FirstNode;

    public void Enqueue(T value){
        // This is a bit more complicated. You need to let the last Node in the Queue know who's next after him.
        // No other choice but looping through your Nodes until you reach the end.
        // You know that you've reached the end, if the current Node's Next Node is null.
        // Then, you assign a new Node containing the value to the current node's Next field.

        // Analogy: In our store, we always remember who's the first that arrived. When a new customer arrives, we tell the last customer, that the new customer will be after them.
        // However, we only know, who's the first customer. And each customer knows, who comes after them. So we continue asking each customer, who comes after them, until one says: "No one! I'm last in the Queue" and we can tell them: "Not anymore! This new customer is now last in the queue"
    }

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