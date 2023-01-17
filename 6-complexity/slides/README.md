# 6 Complexity
When handling collections, we often discuss an operation's complexity.
- We will learn the details in Part 2, Sorting.
- But in Short: The more elements in a collection we need to iterate over for a certain operation, the higher its complexity is.
  - O(1) means, that we can can always complete the operation without running over all nodes.
    - e.g. our `TurboLinkedQueue`'s `Dequeue` Function can directly dequeue the Node without running through all Nodes.
  - O(n) means, that we need to iterate over all nodes in order to complete an operation.
    - e.g. our `TurboLinkedQueue`'s `Count` Property needs to iterate over all nodes until it can say for sure, how many there are.
    - do you have an idea, how you could improve that?
    - implement it!

The solution to such problems is usually to cache some information. We often trade Complexity for Storage. In this case, the solutions are quite easy.

---