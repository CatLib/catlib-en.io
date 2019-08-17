---
title: 有序集
---

# 有序集

An ordered collection is also a collection of elements, like a collection, and does not allow duplicate members. The difference is that each element is associated with a score that can be sorted.

An ordered collection is a fractionation of the members of the collection from small to large. The members of an ordered collection are unique, but the scores can be repeated.

> In the following sample code: `foo/10` means `TElement` is `foo`, `TSource` is `10`

## Add

Add an element to the ordered collection, or update its score if it already exists and sort it to the correct location.

```csharp
// [ "a/0", "b/5", "c/10" ]
set.Add("foo", 3);
// [ "a/0", "foo/3", "b/5", "c/10" ]
```

## Contains

Determines whether the specified element exists in the sort set.

```csharp
// [ "bar/0", "foo/5", "baz/10" ]
set.Contains("foo");
// true
```

## GetScore

Returns the score of the specified element in the sort set, or `KeyNotFoundException` if the element does not exist.

```csharp
// [ "bar/0", "foo/5", "baz/10" ]
set.GetScore("foo");
// 5
```

## GetRangeCount

Get the number of elements in the fractional interval. (contains `min` or `max`)

``` csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
set.GetRangeCount(10, 100);
//  2
```

## Remove

Removes the specified element from the ordered collection and returns `false` if the removal fails.

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
set.Remove("c");
// true
// [ "a/0", "b/10", "d/101" ]
```

## RemoveRangeByRank

Removes all elements within the range based on the specified `rank range` and returns the number of removed elements. (contains `startRank` or `stopRank`)

> Ranked at 0

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
set.RemoveRangeByRank(1, 2);
// 2
// [ "a/0", "d/101" ]
```

## RemoveRangeByScore

Removes all elements within the range based on the specified `score range` and returns the number of removed elements. (contains `startScore` or `stopScore`)

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
set.RemoveRangeByRank(10, 100);
// 2
// [ "a/0", "d/101" ]
```

## GetRank

Gets the ranking of the specified element. The sort set members are sorted by Score **from small to large** by default (if the custom comparison function is based on the custom comparison function order). If you return `-1`, it means that no elements were found.

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
set.GetRank("c");
// 2
```

## GetRevRank

Gets the reverse ranking of the specified element. The sort set members are sorted by Score **from big to small** by default (if the custom comparison function is based on the reverse order of the custom comparison function). If you return `-1`, it means that no elements were found.

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
sorset.GetRevRank("c");
// 1
```

## GetElementRangeByRank

According to the **ranking**, the list of elements in the range is obtained, and the sorting is from small to large according to the ranking (given the custom comparison function according to the order of the custom comparison function). (contains `startRank` or `stopRank`)

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
set.GetElementRangeByRank(1, 2);
// [0] : "b/10"
// [1] : "c/95"
```

## GetElementRangeByScore

According to the `score` get the list of elements in the range, the sorting is from small to large (the custom comparison function is given the custom comparison function order). (contains `startScore` or `stopScore`)

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
set.GetElementRangeByScore(10, 100);
// [0] : "b/10"
// [1] : "c/95"
```

## GetElementByRank

Get the elements according to the ranking, sorting according to the ranking **from small to large** (given the custom comparison function according to the custom comparison function order)

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
set.GetElementByRank(2);
// "c/95"
```

## GetElementByRevRank

Get elements based on rankings, sorted by size **from large to small** (given custom comparison functions in reverse order of custom comparison functions)

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
set.GetElementByRevRank(2);
// "b/10"
```

## GetIterator

To get an iterator, you need to pass in a value to determine whether it is a preorder traversal.

```csharp
// physical order: [ "a/0", "b/10", "c/95", "d/101" ]
// foreach order: [ "a/0", "b/10", "c/95", "d/101" ]
set.GetIterator();

// physical order: [ "a/0", "b/10", "c/95", "d/101" ]
// foreach order: [ "d/101", "c/95", "b/10", "a/0" ]
set.GetIterator(false);
```

## First

Get the first element.

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
set.First();
// "a/0"
```

## Last

Get the last element.

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
set.Last();
// "d/101"
```

## Shift

Removes and returns the elements of the sort set header.

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
set.Shift();
// "a/0"
// [ "b/10", "c/95", "d/101" ]
```

## Pop

Removes and returns the elements at the end of the sort set.

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
set.Pop();
// "d/101"
// [ "a/0", "b/10", "c/95" ]
```

## this[int]

Get the element of the specified ranking.

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
set[2];
// "b/10"
```

## ToArray

Convert an sort set to an array.

```csharp
// [ "a/0", "b/10", "c/95", "d/101" ]
set.ToArray();
// [ "a", "b", "c", "d" ]
```