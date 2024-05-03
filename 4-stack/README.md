# 4 Stack (Linked)

## 4.1 Tests
- Add a new class named `TurboLinkedStackTests` to `TurboCollections.Test`
- For each method that you implement in `4.2`, write reasonable Unit Test Cases.

## 4.2 Class (C#)
- Add a new File named `TurboLinkedStack.cs` to your Project `TurboCollections`.
- In that File, implement the class documented below:

```cs
using System.Collections;

public class TurboLinkedStack<T> : IEnumerable<T> {
    class Node {
        public T Value;
        public Node Previous;
    }
    Node LastNode;

    public void Push(T item) {
        throw new NotImplementedException();
        // Insert Code from AddNumber Example in #4 here
    }

    public T Peek() {
        throw new NotImplementedException();
        // Return the Value of Last Node here.
    }

    public T Pop()
    {
        throw new NotImplementedException();
        // 1. Save the Last Node locally so we can return the value later.
        // 2. Now, assign the Last Node's Previous Node to be the Last Node.
        // -- This effectively removes the previously Last Node of the Stack
        // -- Imagine LastNode is customer 436
        // -- -- who remembered that customer 435 was before him.
        // -- We assign that before customer 435 to LastNode.
        // -- -- 435 knows that 434 was before him.
        // -- -- But he has no memory of customer 436.

        // Now, return the Value of the Node that you cached in Step 1.
    }

    public void Clear() {
        // This one is incredibly easy. Just assign null to Field LastNode
        // -- This is like pretending you never new that there is any last customer.
        // -- by forgetting the latest customer, you forget them all.
    }

    public int Count {
        get{
            // Here, you need to do a while loop over all nodes
            // Similar to the previous PrintAllNodes Function
            // But instead of Printing Nodes, you just count how many Nodes you have visited
            // Similar to this:
            int count = 0;
            while(false/* remove false and replace with correct condition...*/){
                count++;
            }
            return count;
        }
    }

    public IEnumerator<T> GetEnumerator() {
        // This one is a bonus and a bit more difficult.
        // You need to create a new class named Enumerator.
        // You find the details below.
        var enumerator = new Enumerator(LastNode);
        return enumerator;
    }
    
    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }

    class Enumerator : IEnumerator<T> {
        private Node CurrentNode;
        private Node FirstNode;
        private Node _lastNode;

        public Enumerator(Node lastNode) {
            _lastNode = lastNode;
        }

        public bool MoveNext(){
            throw new NotImplementedException();
            // if we don't have a current node, we start with the first node
            if(CurrentNode == null){
                CurrentNode = FirstNode;
            } else {
                // Assign the Current Node's Previous Node to be the Current Node.
            }
            // Return, whether there is a CurrentNode. Else, we have reached the end of the Stack, there's no more Elements.
        }

        public T Current {
            get{
                throw new NotImplementedException();
                // Return the Current Node's Value.
            }
        }

        // This Boiler Plate is necessary to correctly implement `IEnumerable` interface.
        object IEnumerator.Current => Current;

        public void Reset() {
            // Look at Move. How can you make sure that this Enumerator starts over again?
        }

        public void Dispose()
        {
            // This function is not needed right now.
        }
    }
}
```

The Iterator Pattern code can actually be heavily reduced if you understand how to use the `yield`-Keyword correctly:

```cs
using System.Collections;

public class TurboLinkedStack<T> : IEnumerable<T> {
    class Node {
        public T Value;
        public Node Previous;
    }
    Node LastNode;

    public void Push(T item) {
        throw new NotImplementedException();
        // Insert Code from AddNumber Example in #4 here
    }

    public T Peek() {
        throw new NotImplementedException();
        // Return the Value of Last Node here.
    }

    public T Pop()
    {
        throw new NotImplementedException();
        // 1. Save the Last Node locally so we can return the value later.
        // 2. Now, assign the Last Node's Previous Node to be the Last Node.
        // -- This effectively removes the previously Last Node of the Stack
        // -- Imagine LastNode is customer 436
        // -- -- who remembered that customer 435 was before him.
        // -- We assign that before customer 435 to LastNode.
        // -- -- 435 knows that 434 was before him.
        // -- -- But he has no memory of customer 436.

        // Now, return the Value of the Node that you cached in Step 1.
    }

    public void Clear() {
        // This one is incredibly easy. Just assign null to Field LastNode
        // -- This is like pretending you never new that there is any last customer.
        // -- by forgetting the latest customer, you forget them all.
    }

    public int Count {
        get{
            // Here, you need to do a while loop over all nodes
            // Similar to the previous PrintAllNodes Function
            // But instead of Printing Nodes, you just count how many Nodes you have visited
            // Similar to this:
            int count = 0;
            while(false/* remove false and replace with correct condition...*/){
                count++;
            }
            return count;
        }
    }

    // Iterate over all nodes and yield return the current node's value
    public IEnumerator<T> GetEnumerator() {
        throw new NotImplementedException();
        Node current = LastNode;
        // Now, while the current node is not null, do:
        //     yield return the current node's value
        //     then, assign the current node's previous Node to current
    }
    
    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }
}
```


## 4.2 Class (C++)
- Add a new File named `TurboLinkedStack.h` to your Project `TurboCollections`.
- In that File, implement the class documented below:

```cs
template <typename T>
class TurboLinkedStack<T> {
    struct Node {
        T Value;
        Node* Previous;
    };
    Node* lastNode;
public:
    void push(const T& item) {
        throw "NotImplementedException()";
        // Insert Code from AddNumber Example in #4 here
    }

    T& top() {
        throw "NotImplementedException()";
        // Return the Value of Last Node here.
    }

    const void pop()
    {
        throw "NotImplementedException()";
        // Assign the Last Node's Previous Node to be the Last Node.
        // -- This effectively removes the previously Last Node of the Stack
        // -- Imagine LastNode is customer 436
        // -- -- who remembered that customer 435 was before him.
        // -- We assign that before customer 435 to LastNode.
        // -- -- 435 knows that 434 was before him.
        // -- -- But he has no memory of customer 436.
    }
    
    bool empty() const {
        throw "NotImplementedException()";
        // Return true, if the LastNode is null
    }

    size_t size() const {
        throw "NotImplementedException()";
        // Here, you need to do a while loop over all nodes
        // Similar to the previous PrintAllNodes Function
        // But instead of Printing Nodes, you just count how many Nodes you have visited
        // Similar to this:
        size_t count = 0;
        while(false/* remove false and replace with correct condition...*/){
            count++;
        }
        return count;
    }

    template<typename T>
    class Iterator{
        Node* currentNode;
        
    public:
        Iterator(Node* startAtNode) {
            currentNode = startAtNode;
        }

        Iterator<T>& operator++() {
            throw "NotImplementedException()";
            // Make currentNode point to currentNode's previous node
            return *this;
        }

        T& operator*() const {
            throw "NotImplementedException()";
            // Return the value of the CurrentNode
        }

        bool operator==(const Iterator<T>& other) const {
            throw "NotImplementedException()";
            // return true if both operators have the same current node
        }
        bool operator!=(const Iterator<T>& other) const {
            throw "NotImplementedException()";
            // return true if both operators have different current nodes
        }
    };

    Iterator<T> begin() {
        throw "NotImplementedException()";
        return Iterator<T>(LastNode);
    }
    
    Iterator<const T> begin() const {
        throw "NotImplementedException()";
        return Iterator<const T>(LastNode);
    }
};
```


## 4.3 Application

Write a `GameStateHistory` - Console Application in which multiple states exist. 
- From Main Menu, you can go to Level 1, Settings or Quit.
- From Settings you can not go anywhere
- On Quit the Application Quits
- From Level 1, you can go to Level 2 or the Main Menu.
- From Level 2, you can go to Level 3 or the Main Menu
- etc.

Reference your `TurboCollections`-Project and use your `TurboLinkedStack`!

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
