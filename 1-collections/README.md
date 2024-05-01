# 0 Introduction

## Solution
- Create an empty solution named `Algorithms-And-DataStructures`

## Console: HelloWorld
- Create a Console Project within the Solution named `HelloWorld`
- Run the Project

## Library: TurboCollections
- Create a Library Project within the Solution named `TurboCollections`

### Class: TurboMaths

#### C#
- In that Library Project, Create a `public static class` named `TurboMaths`
  - Make sure that it's within the `namespace` `TurboCollections`
  - With a `public static` Method `SayHello()`
  - Have it print `$"Hello, I'm {typeof(TurboMaths).FullName}"` to the Console

#### C++
- In that Library Project, Create a `public class` named `TurboMaths`
  - delete the parameter-less constructor: `TurboMaths() = delete;`
    - That's the C++ way of making a class `static` (un-instantiable)
  - Make sure that it's within the `namespace` `TurboCollections`
  - With a `public static` Method `SayHello()`
  - Have it print `"Hello, I'm " << typeid(TurboMaths).name() << "\n"` to the Console

## Unit Tests: TurboCollections.Test
- Create a Unit Test Project within the Solution using `NUnit` named `TurboCollections.Test`
- Add `TurboCollections` as a Reference to your `TurboCollections.Test`-Project

### Tests: TurboMathsTests
- In that Unit Test Project, Create a `public static class` named `TurboMathsTests`
  - Add a Unit Test named `SayHelloExists()`
  - Have it invoke the member method `SayHello` of class `TurboMaths` in namespace `TurboCollections`
  - Then have it run `Assert.Pass()` to explicitly pass the test
- Add `TurboCollections` as a Reference to your `HelloWorld`-Project
- Edit the `Program` class within your `HelloWorld`-Project.
  - Invoke the member method `SayHello` of class `TurboMaths` in namespace `TurboCollections` instead of printing `"Hello, World!"`
- Run the Project

# 1 Collections
- Create a Project named `P1_2_Collections`
- Create a `List` to store the numbers `137`,`1000`, `-200`
- Use a `for`-Loop and the `index-Operator []` to print all values in the `List`
- Create an `ArrayList` to store the values `true`, `"Forsbergs"`, `'a'`, `1000`, `.12f`;
- Use a `for`-Loop and the `index-Operator []` to print all values in the `ArrayList`