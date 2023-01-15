# 6 Complexity

## Performance Optimization: Improving Count
- Assess `TurboLinkedQueue`'s `Count()` Complexity
  - For n items in the collection, how many Items need to be iterated over in order to return the correct Count
- Improve `TurboLinkedQueue`'s `Count()` Complexity
  - Cache the `Count` in a Field instead of calculating it dynamically

## Performance Optimization: Improving Enqueue
- Assess `TurboLinkedQueue`'s `Enqueue(T value)` Complexity
  - For n items currently in the collection, how many Items need be iterated over in order to enqueue a new element at the back of the Queue?
- Improve `TurboLinkedQueue`'s `Enqueue(T value)` Complexity

```cs
public class TurboLinkedQueue<T> : IEnumerable<T> {
    /*...*/
    // We always store and update the LastNode as well.
    Node LastNode;

    public void Enqueue(T value){
        // Now, we just need to check, if there is a last Node.
        // Then assign the New Node as the Last Node's Next Node.
        // And assign the New Node as the Last Node.

        // Analogy: In our store, we always remember, who's the last customer that entered. So if a new customer arrives, we know, whom to tell who is his next customer.
    }
    /*...*/
```