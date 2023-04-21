---
title: A quick tour of dictionaries in C#
published: true
description: 'A quick tour of dictionaries in C#, including where to use them'
tags: 'csharp, dotnet, dictionaries, beginner'
cover_image: ../assets/dev/cSharpDictionaries/dictionaries.png
canonical_url: null
id: 1443808
date: '2023-04-21T19:10:53Z'
---

## Preface

Dictionaries in C# are one of the more useful data structures you can use to store and retrieve data quickly and most programming languages have some form of dictionary built into them.  However, there are a lot more versions of dictionary in C# and I haven't an article bringing them all together to discuss where you'd use one over the other, so hopefully this might help you out.

## What's a dictionary?

Put simply, a dictionary is a list of key-value pairs.  It's mainly used when you need to store large or complex objects within the code while also optimizing for quick retrieval of data. It does this by allowing you to pull out a key (which has to be unique) that is linked to the larger object (the value).  However, while this was original intent, dictionaries are regularly used for 'simpler' retrieval such as pairs of strings as they are also useful guaranteeing uniqueness in a collection.  Dictionaries are also (usually) part of the generic namespace in C# because the type of the key and value is declared at initialization.

## Dictionary types

As I mentioned earlier there's a fair number of *types* of dictionary in C# so let's go through them now.

### Dictionary

This is the "default" dictionary.  It's pretty decent in most scenarios where you need one and is also the 'oldest' form of dictionary in C#.  Given this is the default, I'm going to run through some common features of a dictionary within C#.

You can declare this dictionary like this:

```csharp
Dictionary<int, string> myDictionary = new Dictionary<int, string>();
```

This form of dictionary allows you to add and remove entries at will:

```csharp
myDictionary.Add(1, "stuff");

myDictionary.TryAdd(1, "stuff");

// You only remove via the key
myDictionary.Remove(1);

myDictionary.Clear();
```

you can find items in a dictionary in a few different ways (though I'd probably avoid linq):

```csharp
myDictionary.ContainsKey(1);
string value = myDictionary[1];

// This is actually coming from the IReadOnlyDictionary interface
myDictionary.GetValueOrDefault(1);

myDictionary.TryGetValue(1, out string? value);

myDictionary.Where(m => m.Key == 1).First();
```

You probably noticed there's a few methods available are prefixed by `Try`.  This is because the default of a dictionary is to throw an `ArgumentException` if the method does not return positively. As a result, C# provides some methods that allow you to *safely* retrieve values without having to resort to try/catch blocks which can be slow.

Interestingly, because dictionaries are generic, you can actually chain them together like this:

```csharp
Dictionary<int, Dictionary<string, string>> myChainedDictionary = new Dictionary<int, Dictionary<string, string>>();
```

This is probably something I wouldn't recommend outside some very specific situations, but it's possible.

Finally, a dictionary is not thread-safe by default, but you can introduce this feature using locks.

### Read-Only Dictionary

A read only dictionary works the same way under the hood as a normal dictionary only that values can only be added during initialization.  What this means, is that there are no `add` and `remove` methods available.

There are actually 2 ways a read only dictionary can be declared, although one of them is via an interface:

```csharp
IReadOnlyDictionary<int, string> myReadOnlyDictionary = new Dictionary<int, string>()
{
    { 1, "stuff" }
};

// This lives in System.Collections.ObjectModel
ReadOnlyDictionary<int, string> myReadonlyDictionary = new ReadOnlyDictionary<int, string>(myDictionary);
```

There is a small difference between these methods as the `ReadOnlyDicitonary` has a `TryAdd` method, but attempting to call it will cause a `NotSupportedException` to be thrown.

The main *reason* to have these methods available is when you want to make data in a dictionary available to callers, but don't want to allow a caller to modify the collection.

### Immutable Dictionary

An immutable dictionary works in a very similar manner to the standard dictionary in C# *but* whenever you `add` or `remove` an item from the dictionary, it creates a copy of the dictionary, adds the item and then returns the new collection to you.  This means that the underlying object stays the same and therefore an immutable dictionary is `thread-safe` compared to previous entries in this list.  However, because new dictionaries are created every time an immutable dictionary is updated, there is a performance impact that isn't found on a standard dictionary.

This dictionary type also lives in the `System.Collections.Immutable` namespace.

### Sorted Dictionary

The sorted dictionary is really quite straightforward, it will *sort* the dictionary based on the value of the key.

I've written a DotNetFiddle showing this in practice below:

<!-- markdownlint-disable-next-line -->
{% dotnetfiddle https://dotnetfiddle.net/CDPsuT %}

Sorted dictionaries are really only useful when you need the data to be sorted (an example I've seen is using DateTime as a key and needing to retrieve anything between 2 dates).  This is because a sorted dictionary is much slower than a standard dictionary due to the increased overhead sorting has when adding and fetching items.  There are more details on this found [here](https://www.dotnetperls.com/sorteddictionary).

### Concurrent Dictionary

There's a really good in-depth article found [here](https://code-maze.com/concurrentdictionary-csharp/) around how a concurrent dictionary works, but essentially it's a dictionary that's designed to run safely across multiple threads (as described by the "concurrent" part).  However, it works very differently from an immutable dictionary and does not return a new dictionary every timer a value is added.  The concurrent dictionary was added fairly recently to C# and it's what I'd recommend if you need to access to a shared dictionary.

Frozen dictionaries do have a really useful method called `AddOrUpdate` which will attempt to add a value to the dictionary, if the key already exists, it will update the value.  This method uses function within itself to choose whether to update the value, or keep it the same

Here's an example of this:

```csharp
ConcurrentDictionary<int, string> concurrentDictionary = new ConcurrentDictionary<int, string>();

concurrentDictionary.AddOrUpdate(1, "value", (key, currentValue) => currentValue);
concurrentDictionary.AddOrUpdate(1, "someValue", (key, currentValue) => "valueTwo");
concurrentDictionary.AddOrUpdate(2, "someValue", (key, currentValue) => "valueTwo");
concurrentDictionary.AddOrUpdate(3, "value", (key, currentValue) => "valueTwo");
concurrentDictionary.AddOrUpdate(3, "someValue", (key, currentValue) => currentValue);

// this results in the following dictionary:
// key: 1, value: valueTwo
// key: 2, value: someValue
// key: 3, value: value
```

The concurrent dictionary lives in the `System.Collections.Concurrent` namespace

### Frozen Dictionary

As of writing this article, frozen dictionaries are brand new to C#.  They share a lot of similarities between a read-only dictionary, in that once a frozen dictionary has been initialized, new values cannot be added to them.  However, there's a major difference between the two in that frozen dictionaries are heavily optimized for read speed as opposed to the read-only dictionary, which is closer to a wrapper on the normal dictionary class.

What this means, is that frozen dictionaries take longer to initialize than a regular dictionary, but retrieving values is much faster.  There's a comment [here](https://github.com/dotnet/runtime/issues/67209#issuecomment-1298780079) on the GitHub repository for .Net showing that a frozen implementation of hash set was over twice as quick to get a value, as compared to a regular hash set. As a consequence, the use case for a frozen dictionary is for long-lived collections of data that doesn't change.

The frozen dictionary lives in the `System.Collections.Frozen` namespace.

### Hash Table

OK, a hash set isn't *technically* a dictionary, but it performs a function similar enough that worth mentioning in this article as they are essentially, a collection of key-value pairs that require unique keys.

The first difference between a hash table and a dictionary is that they aren't generic, they instead accept *objects*. The difference is subtle when you're new to C#, but basically, generics are quicker and also signpost errors better than when you're using objects.

This means a hash table can be declared like this:

```csharp
Hashtable myHashTable = new Hashtable();
```

Frankly, while hash tables exist in C#, I would be cautious of using them given how dangerous they can be around typing.  For example:

```csharp
myHashTable.Add(1, "stuff");

// this line will throw an error at runtime
// myHashTable.Add(1, "stuffTwo");

// this won't
myHashTable.Add("1", "stuffTwo");

// this results in the following hash table:
// key: 1, value: stuff
// key: "1", value: stuffTwo
```

As you can see, you can use multiple similar keys with a different type in a hash table, and most crucially, *it doesn't complain when you get the type wrong*. This can become a serious issue when you start objects throughout the code, which can be less obvious on if the types match.

The hash table can be found in the `System.Collections` namespace.

### List of Tuples

This is most definitely not a dictionary, but it still stores 2 (or more) values in a list.  This should be used when all values are of equal importance, and you actually want the duplicates in the list.  To be absolutely clear, a tuple is *not* a key-value pair, it's better described as value-value pair and can in fact be several values (i.e.: value-value-value-value etc).  A contrived example of using a list of tuples could be stock tracking system where all you care about is what's been sold and how much.  In this case you'd want to know that you'd had 10 socks leave the warehouse on several occasions, rather than that somebody had removed 10 socks from the warehouse on at least 1 occasion that a dictionary would give you.

A list of tuples can also use *named* values which makes it easier to keep track of what the values mean, as compared to a dictionary which is always named `key` and `value`.  For example:

```csharp
List<(int number, string item)> myTupleList = new List<(int, string)>();

myTupleList.Add((10, "socks"));

int numberOfFirstItem = myTupleList[0].number;
```

I also find tuples useful when you want to return more than 1 object from a method, without having to resort to the `out` parameter in C#.
