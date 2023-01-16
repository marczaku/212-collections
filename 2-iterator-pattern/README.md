# 2 Iterator Pattern
- Create a Project named `1_3_Iterator`
- Create a `List` with 5 numbers: `1`, `1`, `2`, `3`, `5`.
- Assign it to a Variable of Type `IEnumerable`.
- Use `GetEnumerator()` and a `while`-Loop to print all elements of the `IEnumerable`-Variable.
- Can you also add up all numbers of the List using `IEnumerable` and then print the sum? If not, then what do you need to change?
- Implement the functions `GetOddNumbers` and `GetOddNumbersList` documented below within your `TurboMaths` class
  - Hint: use `yield` within `GetOddNumbers`
  - Add Unit Tests for both Functions (Use the test cases documented above the function as a base)
- Invoke each functions within `Program` in your `1_3_Iterator` Project and pass `12` as an argument
- Use `foreach` to iterate over the result and print each number of the result
- Invoke `GetOddNumbers` with `1_000_000_000` as an argument. No Problem, right?
- Invoke `GetOddNumbersList` with `1_000_000_000` as an argument. Observe the result.

```cs
// Returns all Odd Numbers until the given number. e.g.:
// GetEvenNumbersList(12) -> {0, 2, 4, 8, 10, 12};
// GetEvenNumbersList(15) -> {0, 2, 4, 8, 10, 12, 14};
// GetEvenNumbersList(-1) -> {};
// GetEvenNumbersList(0) -> {0};
public static List<int> GetEvenNumbersList(int maxNumber){

}

// Returns all Odd Numbers until the given number. e.g.:
// GetEvenNumbers(12) -> {0, 2, 4, 8, 10, 12};
// GetEvenNumbers(15) -> {0, 2, 4, 8, 10, 12, 14};
// GetEvenNumbers(-1) -> {};
// GetEvenNumbers(0) -> {0};
public static IEnumerable<int> GetEvenNumbers(int maxNumber){

}
```