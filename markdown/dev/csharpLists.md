---
title: A quick tour of lists in C#
published: true
description: 'A quick tour of lists in C#, including some of the reasons to use them'
tags: 'csharp, dotnet, lists, beginner'
cover_image: ../assets/dev/csharpLists/lists.png
canonical_url: null
id: 1451656
date: '2023-04-28T20:29:47Z'
---

## Preface

Some of you might have seen my [previous article](https://dev.to/jlewis92/a-quick-tour-of-dictionaries-in-c-gg9) around dictionaries in C#.  Seemed to be fairly popular, so I thought I'd write a similar around lists in C#.  AS in dictionaries, C# provides a lot of versions of a simple list.  I've not seen previous articles comparing and contrasting the different versions of a list and where to use them.

## What's a list?

Quite simply, a list is a data structure in C# used to hold list of objects of the same type.  They are also generic, meaning the type in the list is declared at initialization.  Lists are also specifically an *ordered* data structure, meaning that the order you add items to the list is the order of the items in that list, including duplicate values.  Finally, lists are "designed" to be throwaway.  This is because it can be difficult to extend a list in the same way a collection can be. In fact, while collections are outside the scope of this article, it's generally considered better practice to return a collection from public methods, but in reality a list works fine 999 times out of 1000.

There is more information on lists found [here](https://learn.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1?view=net-8.0).

## Types of list

As in the previous article, I'll run through some of the features I use most often in the "default" list and move onto other implementations afterwards.

**Note:** I'm using nullable strings (i.e.: `string?`) throughout this article to make it more clear.

### List

Lists can be declared like the following example using a list of strings:

```csharp
List<string> stringList = new List<string>();
```

In general, data can be added, inserted or removed from the list like the following:

<!-- markdownlint-disable-next-line -->
{% dotnetfiddle https://dotnetfiddle.net/8CuFMQ %}

There are a *lot* of methods that can be used to find things in lists.  Here's a few of the more common ones.

```csharp
// The simplest way to find an item
// returns null if it's not in the list
string? foundString = stringList.Find(s => s == "some string");
string? nullString = stringList.Find(s => s == "this is not in the list");

// If you want more than one, you can use .Where:
IEnumerable<string> multipleFound = stringList.Where(s => s.Contains("string"));
// As a list:
List<string> multipleFoundAsList = stringList.Where(s => s.Contains("string")).ToList();
// Returns an empty list if it finds nothing
List<string> noneFound = stringList.Where(s => s.Contains("nothing here")).ToList();

// There are also methods like select, which checks whether the item matches
// and then returns a bool based on the match.
IEnumerable<bool> test = stringList.Select(x => x == "first string");

// you can find by index like this:
string firstItem = stringList[0];
//string throwsArgumentOutOfRangeException = stringList[4];

// You cans earch for first or last directly as well
string anotherFirst = stringList.First();
// If you don't want it to throw an error for an empty list
string? lastItem = stringList.LastOrDefault();

// There are also several ways to go in the opposite direction:
int firstIndex = stringList.IndexOf(firstItem);
int anotherWayTofindIndex = stringList.FindIndex(s => s == firstItem);
// If you want to find the last occurrence in the list:
int lastIndex = stringList.LastIndexOf(firstItem);
int anotherWayTofindLastIndex = stringList.FindLastIndex(s => s == firstItem);

// There are also binary searches, but these should ONLY be used on sorted lists
int firstIndex = stringList.BinarySearch("first string");
```

You can sort lists with the `Sort` method and it's overloads.

Finally, there are options around copying lists:

```csharp
List<string> stringList = new List<string>()
{
    "first string",
    "second string",
    "third string"
};

string[] stringArray = new string[3];

// Copy as an array
stringList.CopyTo(stringArray);

// copy as a list
List<string> newStringList = stringList.GetRange(0, stringArray.Length);
```

The general list is available via the `System.Collections.Generic` namespace.

### Read-only List

A read-only list, is a list that cannot have items added to it after initialization.  It's usually used when data has to be passed to a calling method, but you don't want the calling code to have access to change the list.

As in the read-only dictionary, a read-only list can be declared like the following:

```csharp
IReadOnlyList<string> newList = new List<string>();
```

You can also create a read-only collection from a list like the following:

```csharp
IReadOnlyCollection<string> readOnlyCollection = stringList.AsReadOnly();
```

### Linked List

At the top level, a linked list works the same way as a generic list.  However, it also contains additional methods to place items *relative* to a position in the list. As a result, if you need to retrieve or add items relative to an item in the list, they work better.  Otherwise, it's better to use the normal `List<T>` class.

Examples of some additional methods in a linked list are:

```csharp
// Adding
LinkedList<string> linkedStringList = new LinkedList<string>(new List<string>()
{
    "second item"
});

linkedStringList.AddFirst("first item");
linkedStringList.AddLast("last item");

LinkedListNode<string>? linkedListNode = linkedStringList.First;
linkedStringList.AddAfter(linkedListNode, "actually second item");
linkedStringList.AddBefore(linkedStringList.First, "actually first item");

// Removing
linkedStringList.RemoveFirst();
linkedStringList.RemoveLast();
```

Linked lists are also available via the `System.Collections.Generic` namespace.

### Immutable List

An immutable list is a list that returns a new version of the list whenever an item is added or removed.  This means that the immutable list is `thread-safe` provided you use `lock` functionality in C#.  However, because the list creates an entirely new list every time items are added or removed, it can provide a performance hit over using a standard list.

Immutable lists are a little strange, in that you don't create them the same way as a normal list as they don't have a public constructor:

```csharp
ImmutableList<string> immutableStringList = ImmutableList<string>.Empty;
```

As I said earlier, add and remove actually create new versions of the immutable list:

```csharp
ImmutableList<string> newImmutableStringList = immutableStringList.Add("first");
```

### Sorted List

A sorted list is a list that uses the `IEqualityComparer` to sort the list. It *only* supports key-value pairs and can be declared like the following:

```csharp
SortedList<string, string> sortedStringList = new SortedList<string, string>();
```

In the same way that a Dictionary works, you cannot add duplicate keys to the list. However, where it differs is that it sorts lists based on the *value*.  Here's an example of this in action:

<!-- markdownlint-disable-next-line -->
{% dotnetfiddle https://dotnetfiddle.net/aOKAPz %}

Sorted lists also live in the `System.Collections.Generic` namespace.

### Stacks and Queues

Really, there's enough variations of stacks and heaps that they require their own article.  However, hey still are *technically* lists as they maintain the order of a collection, just in slightly different ways. It's best to say that stacks and heaps in C# closely implement the [stack and heap](https://hackr.io/blog/stack-vs-heap) data structures.

#### Stack

While it's outside the scope of this article, a stack is a first-in-last-out data structure.

If you're used to JavaScript, the syntax should be fairly familiar:

```csharp
Stack<string> stackStringList = new Stack<string>();

// Add
stackStringList.Push("first");
stackStringList.Push("second");
stackStringList.Push("third");

// Check
string third = stackStringList.Peek();
stackStringList.TryPeek(out string? alsoThird);

// Remove
string thisIsthird = stackStringList.Pop();
stackStringList.TryPop(out string? second);
```

#### Queue

Queues are essentially the opposite of a stack, in that they are first-in-first-out.

You can use them like the following:

```csharp
Queue<string> queueStringList = new Queue<string>();

queueStringList.Enqueue("first");
queueStringList.Enqueue("second");
queueStringList.Enqueue("third");

string first = queueStringList.Dequeue();
```

Both the default stack and queue live in the `System.Collection.Generics` namespace.

### Array

An array is *also* an ordered list of objects.  However, the big difference is that you need to declare how long an array is at initialization:

```csharp
string[] arrayOfFiveEmptyString = new string[5];

string[] arrayOfThree = { "one", "two", "three" };

// changing the array to a length of five
Array.Resize(ref arrayOfThree, 5);
```

Arrays are older data structures in C#, and while they are absolutely used within C# (such as a byte array), it's usually preferable to use a list due to the increased flexibility as arrays throw errors if you try to assign values outside the array's length.

Arrays are fundamental to the operation of C# and as such live in the `System` namespace.

### Array List

An array list is the *forerunner* to a modern list from the times before C# had generics.  As such an array list accepts a list of objects, rather than a generic.  This means that you do not benefit from type safety and as such, should be used with caution.

For example, the following code does not throw errors:

```csharp
ArrayList arrayList = new ArrayList();

arrayList.Add("1");
arrayList.Add(1);
```

Array lists live within the `System.Collections` namespace

### Tag List

Tag lists are a little strange, in that they are `structs` and like the sorted list, accept key-value pairs. However, essentially they're used when you need to optimize for memory usage as they avoid allocating memory when you have 8 or fewer items in the collection.  They also accept objects in the value field, rather than generics, meaning that you don't have the advantage of type safety when using them.

```csharp
TagList tagList = new TagList();
tagList.Add("tag", "1");
tagList.Add("tagTwo", 1);

var tag = tagList.First(t => (string?)t.Value == "1");
// throws an exception
var exceptionTag = tagList.First(t => (int?)t.Value == 1);

var tagTwo = tagList.Last(t => (int?)t.Value == 1);
```

Tag lists actually live within the `System.Diagnostics` namespace
