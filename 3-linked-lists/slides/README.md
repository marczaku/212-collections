# 3 Linked List (Introduction)

## Problem
Arrays can not be resized. You can only create them with one size and that's it, they can not grow or shrink (There will be another, more efficient solution to this problem later). So Arrays won't work if we want to build a collection that can continuously grow as elements are added.

```cs

int[] numbers = new int[3];
int numberCount = 0;

void AddNumber(int number){
    numbers[numberCount++] = number;
}

int GetNumber(int index){
    return numbers[index];
}

AddNumber(1);
AddNumber(2);
AddNumber(3);
AddNumber(4); // EXCEPTION! Why?
GetNumber(0);
GetNumber(3); // EXCEPTION! Why?
```

### Incomplete Solution: Bigger Array

The solution to above problem could be to just create a bigger Array.

```cs
int[] numbers = new int[100];
```

But what, if we need more than 100 numbers?

### Incomplete Solution: Really Big Array

```cs
int[] numbers = new int[10_000_000];
```

- Still, it could happen that we need more than 100_000 Elements. You can't predict every edge case.
- Also, above array consumes a lot of Memory (40MB)
  - One `int` has 4 bytes
  - We got an array for `10_000_000` Elements
  - `10_000_000 * 4B = 40_000_000B`
    - `40_000_000B / 1024 = 39_062.5 KB`
    - `49_062.5KB / 1024 = 38.15MB`

## Solution
Instead of storing our Elements in an Array, we could create a Linked List.
- We store each Element in a `Node`
- Each `Node` remembers its `Next` or `Previous` Node.

Imagine a Queue in a shop. Instead of every customer receiving a numbered place in the queue (like an index in an array), you just tell every customer to remember who got the spot before them. If you now want to talk to every customer, you just start with the last one and then ask the customer, who had the spot before him. You repeat that process with each customer until one customer says. "That's it, I'm the last one!"

### Part 1: The Node
The Node can store one number as well as a reference to the previous Node:

```cs
public class Node {
    public int Value;
    public Node Previous;
}
```

### Part 2: A Demo for Adding and Printing Numbers

```cs
Node LastNode = null;
void AddNumber(int number){
    LastNode = new Node{
        Value = number,
        Previous = LastNode
    };
}
void PrintAllNumbers() {
    Node CurrentNode = LastNode;
    while(CurrentNode != null){
        Console.Write($"{CurrentNode.Value}, ");
    }
    Console.WriteLine("(END)");
}

for(int i = 0; i < 1000; i++){
    AddNumber(i);
}

PrintAllNumbers();
```

#### Add Number Explained

Above implementation of `AddNumber` does a lot of things within a single Expression. Let's look at a version where each step is split up into its own expression:

```cs
void AddNumber(int number){
    // Create a new Node, so we can store the new number
    Node newNode = new Node();
    // Store the new number to the Node
    newNode.Value = number;
    // Have the Node remember the currently last node as its previous
    newNode.Previous = LastNode;
    // The new node is now the new last node
    LastNode = newNode;
}
```

#### Print Numbers Explained
```cs
void PrintAllNumbers() {
    // We begin by looking at the last Node
    Node CurrentNode = LastNode;
    // If it is not null (that means that there is no more Nodes), then:
    while(CurrentNode != null){
        // We print the value of the current Node
        Console.Write($"{CurrentNode.Value}, ");
        // And ask the node for its previous one, so we can continue with that one.
        CurrentNode = CurrentNode.Previous;
    } // The Loop will jump back here.

    // If the While Loop has ended that means that there is no more Nodes.
    Console.WriteLine("(END)");
}
```

Above kind of collection will the basis for the upcoming Collection Exercises, so make sure that you have understood it well.

---