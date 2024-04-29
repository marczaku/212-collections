# 1 Collections

## Reusable Collections

The goal of most software written is to provide reusable components, in order to prevent us from needing to reinvent the wheel. This was accomplished in .NET over the years in multiple fashion.

## Non-Generic Collections (C#-Only)

Non Generic collections in .NET are old-fashioned collections that provide no Type-Safety. They are included in the `System.Collections`-Namespace.

Look at this example of an ArrayList:

```cs
using System.Collections;

ArrayList list = new ArrayList();
list.Add("Hello");
list.Add(true);
list.Add(-5);
```

You can see, that we can add all kinds of data into this Data Structure. This might not be a problem, if you don't want to limit the kind of information that is stored in this Collection.

But what if you store for example a bunch of numbers and now you want to read them from the Collection? Well, you'll have to cast them first.

```cs
int first = (int) list[0];
int second = (int) list[1];
```

Why is that?

```cs
// Extracted from System.Collections.ArrayList.cs
public virtual int Add(object? value);
// ...
public virtual object? this[int index];
```

It is because this collection only stores information by their base type `object`. Therefore, when accessing any information from the `ArrayList`, you can never be 100% sure, what Type it is and need to do Type-Checking and Type-Casting.

In Summary, Non-Generic Collections are reusable, general-purpose Data Structures provide no Type-Safety, but allow you to store a variable amount of Data.

In C++, a similar result can be achieved by creating a Collection of Type `std::any` (modern C++) or `std::void*` (old-style C++)

## Generic Collections

Generic Collections have been added to .NET when Generic Classes were introduced. They are included in the `System.Collections.Generic`-Namespace.

Look at this example of a List:

```cs
using System.Collections.Generic;

List<string> names = new List<string>();
names.Add("Marc");
names.Add("Daniel");
names.Add("René");
```

Or in C++:

```c++
#include <vector>
#include <string>
using namespace std;

vector<string> names{};
names.push_back("Marc");
names.push_back("Daniel");
names.push_back("René");
```

You can see that now we had to specify a Generic Type Argument for the `List`. This Data Structure will now only allow objects of type `string` to be added to it. Trying to add any other Data Type will result in Compiler Errors.

This also makes accessing the Data very easy:

```cs
using System.Collections.Generic;

List<int> numbers = new List<int>();
numbers.Add(2);
numbers.Add(-5);
int first = numbers[0];
int second = numbers[1];
```

Or in C++:

```c++
vector<int> numbers{};
numbers.push_back(2);
numbers.push_back(-5);
int first = numbers[0];
int second = numbers[1];
```

No Casting necessary. How come?

```cs
// Extracted from System.Collections.Generic.List.cs
public void Add(T item);
// ...
public T this[int index];
```

In other words, Generic Collections are reusable, general-purpose Data Structures that allow you to store a variable amount of Data in a Type-Safe manner.