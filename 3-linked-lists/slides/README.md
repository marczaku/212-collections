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
- Each `Node` stores either its
  - `Next` Node (used in Queue, LinkedList and DoubleLinkedList)
  - or `Previous` Node (used in Stack, DoubleLinkedList)

Imagine a Queue in a shop. Instead of every customer receiving a numbered place in the queue (like an index in an array), you just tell every customer to remember who got the spot behind them. If you now want to talk to every customer, you just start with the first one and then ask the customer, who had the spot behind them. You repeat that process with each customer until one customer says. "That's it, I'm the last one, no one is behind me!"

### Part 1: The Node (C#)
The Node can store one number as well as a reference to the previous Node:

```cs
public class Node {
    public int Value;
    public Node Next;
}
```

### Part 1: The Node (C++)

```c++
struct Node {
    int value;
    Node* next;
};
```

### Part 2: A Demo for Adding and Printing Numbers (C#)

```cs
Node FirstNode = new Node{
    Value = 0,
    Next = null
};
Node LastNode = FirstNode;
void AddNumber(int number){
    var newNode = new Node{
        Value = number,
        Next = null
    };
    Lastnode.Next = newNode;
    LastNode = newNode;
}
void PrintAllNumbers() {
    Node currentNode = FirstNode;
    while(currentNode != null){
        Console.Write($"{currentNode.Value}, ");
        currentNode = currentNode.Next
    }
    Console.WriteLine("(END)");
}

for(int i = 1; i < 5; i++){
    AddNumber(i);
}

PrintAllNumbers();
```

Above kind of collection will the basis for the upcoming Collection Exercises, so make sure that you have understood it well.

### Part 2: A Demo for Adding and Printing Numbers (C++)

```c++
Node* FirstNode = new Node{0, nullptr};
Node* LastNode = FirstNode;
void addNumber(int number){
    Node* newNode = new Node{number, nullptr};
    Lastnode.next = newNode;
    LastNode = newNode;
}
void printAllNumbers() {
    Node* currentNode = FirstNode;
    while(currentNode != nullptr){
        cout << currentNode->value << ", ";
        currentNode = currentNode.next
    }
    Console.WriteLine("(END)");
}

for(int i = 1; i < 5; i++){
    addNumber(i);
}

printAllNumbers();
```