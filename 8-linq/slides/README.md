# 8 LINQ

LINQ, or Language-Integrated-Query, is a technology by .NET which allows to handle Collections like Streams and manipulate them in a similar fashion to how you can manipulate Streams by nesting them. Imagine the following case:

```cs
Animal animals = zoo.GetAnimals();

class Cat : Animal {
    public string FavoriteFood;
    public FoodBowl FoodBowl;
}

class Animal {
    public bool IsHungry;
}

class FoodBowl{
    public void Fill(string food){}
}
```

Now, we notice that we have a lot of tuna left. We want to feed the cats, because they've not eaten in a while. But of course, we only want to feed the cats which are Hungry and like Tuna. Because cats are pretty picky, we have learned that the hard way. Also, they have their favorite FoodBowls. Cats are not easy to please.

We could implement it like this:

```cs
foreach(Animal animal in animals){
    if(animal.IsHungry){
        if(animal is Cat cat){
            if(cat.FavoriteFood == "Tuna"){
                cat.FoodBowl.Fill("Tuna");
            }
        }
    }
}
```

This should work. But what if it's not our Job to actually fill the bowl? We have an assistant that we need to provide with the information, so we should give him a list of all FoodBowls to fill with Tuna:

```cs
List<FoodBowl> foodBowls = new List<FoodBowl>();
foreach(Animal animal in animals){
    if(animal.IsHungry){
        if(animal is Cat cat){
            if(cat.FavoriteFood == "Tuna"){
                foodBowls.Add(cat.FoodBowl);
            }
        }
    }
}
assistant.OrderToFill("Tuna", foodBowls);
```

Neat! But also quite complex. Also, if we have thousands of cats, this will be a very large array again. What if we could Stream the Data to our Assistant? Fact is, we can!

```cs
IEnumerable<FoodBowl> foodBowls = animals
    .Where(animal => animal.IsHungry)
    .OfType<Cat>()
    .Where(cat => cat.FavoriteFood == "Tuna")
    .Select(cat => cat.FoodBowl);

assistant.OrderToFill("Tuna", foodBowls);
```

Pretty cool and expressive, or? And you can do so many more complex things with LINQ: 
- `Concat`: Combine to Collections
- `GroupBy`: Group a Collection by some common Property
- `First`: Get the first item. Exception if none.
- `FirstOrDefault`: Get the first item. Default if none.
- `Single`: Get one element for a condition. Exception if not exactly one.
- `Any`: Returns true, if any item satisfies a condition.
- `OrderBy`: Sort in ascending order.
- `OrderByDescending`: Sort in descending order.
- `ToDictionary`, `ToArray`, `ToList`: Convert to Collection-Type
- ... and many more.

## Query Syntax:
If you're not a fan of symbols, you can even use LINQ's special query syntax in C#:

```csharp
var foodBowls = from animal in animals
                where animal.IsHungry
                let cat = animal as Cat
                where cat != null && cat.FavoriteFood == "Tuna"
                select cat.FoodBowl;

assistant.OrderToFill("Tuna", foodBowls);
```

## Usage:
- AI: Find all buildings of the player. Filter them by only the ones the AI has discovered. Filter them by the ones that seem not too well defended. Sort them by Priority. Attack the First.
- Multiplayer: Show Clan-Members. Filter by the ones being online OR being a High Rank. Sort them by Rank.



# Further Research
- Read up on Linked Lists: Singly, Doubly, Circular
- Implement `TurboLinkedQueue` and `TurboLinkedStack` as Linked Lists.
- Can you think of other situations in which you could use the solution for the Queue-Spoiler?
- Try publish your TurboCollections on NuGet. The name `TurboCollections` is already taken, though :P
- Create a new Project outside of the solution. Add your NuGet-Package using `dotnet add package <packagename>`. Can you use your Library? How does updating the Library work?

---

# Additional Resources
- [TutorialsPoint](https://www.tutorialspoint.com/data_structures_algorithms/index.htm)
- [GeeksForGeeks](https://www.geeksforgeeks.org/data-structures/)
- [Programiz](https://www.programiz.com/dsa)