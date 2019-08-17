---
title: Arr
---

# Arr

Arr helper extends some common operations for arrays.

## Merge

Combine multiple specified arrays into one array.

```csharp
Arr.Merge(new []{ "1", "2" } , new []{ "3" }); 
// [ "1", "2", "3" ]
```

## Rand

Get a specified number of random values from a specified array.

If the number of acquired elements is greater than 1, then the values that have been randomized will not be repeated in the random process.

If the number passed in is greater than the number of elements provided, it will return a `default(T)` null value.

```csharp
Arr.Rand(new []{ "1", "2", "3" }, 2);
```

## Shuffle

Shuffle the elements in the specified array.

```csharp
Arr.Shuffle(new []{ "1", "2", "3" });
```

## Splice

Removes an element of the specified length from the array. If the **replaceSource** parameter is given, the new element is inserted from the **start** position.

```csharp
var data = new []{ "1", "2", "3" };
var count = Arr.Splice(ref data , 1 , 1, new []{ "4", "5" });
// data : [ "1", "4", "5", "3" ]
// count : 4
```

## Chunk

Divide an array into new array blocks.

The number of cells in each array is determined by the **size** parameter. The number of cells in the last array may be a few.

```csharp
Arr.Chunk(new []{ "1", "2", "3" }, 2);
// [["1", "2"], ["3"]]
```

## Fill

Fill the array, if the specified array is passed in, it will be filled based on the specified array.

```csharp
Arr.Fill(2, 3, "100", new []{ "1", "2", "3" });
// [ "1", "2", "100", "100", "100", "3" ]
```

## Remove

Pass each value of the array to the callback function, if the callback function returns true, remove the corresponding element in the array and return the removed element.

```csharp
var data = new string[]{ "1", "2", "3" };
Arr.Remove(ref data, (value) => value == "2");
// [ "2" ]
// data: [ "1", "3" ]
```

## Filter

Each value in the source array is passed to the callback function. If the callback function is equal to the **expected** value, the current value in the input array is added to the result array.

```csharp
Arr.Filter(new[]{ "1", "2", "3", "4", "5" },(value) => (value % 2) == 0);
//  [ "2", "4" ]
```

## Map

Pass the value of the iterator into the callback function, and the value returned by the custom function as the new array value.

```csharp
Arr.Map(new[]{ 1, 2, 3 } , (value) => value * 2);
// [ 2, 4, 6 ]
```

## Pop

Delete the last element in the array and return the deleted element as the return value.

```csharp
var data = new []{ "1", "2", "3" };
var result = Arr.Pop(ref data);
// result : "3"
// data.Length : 2
```

## Push

Add one or more elements to the end of the array.

```csharp
var data = new []{ "1","2" };
Arr.Push(ref data, "3", "4", "5");
// [ "1", "2", "3", "4", "5" ]
```

## Reduce

Pass a value from an array to a callback function and return a string.

The function returns null if the array is empty and the **initial** parameter is not passed.

If the **initial** parameter is specified, the parameter will be treated as the first value in the array, and if the array is empty, it will be the final return value (string).

```csharp
Arr.Reduce(new []{ "1", "2", "3" },(x, y)=> x + "-" + y, "0");
// 0-1-2-3
```

## Slice

Take a value from the array according to the condition and return.

If the value is set to a negative number, the absolute value is taken from the back.

```csharp
Arr.Slice(new []{ "1", "2", "3", "4", "5" }, 1, 3);
// [ "2", "3", "4" ]
```

## Shift

Removed the first element in the array and return the value of the removed element.

```csharp
var data = new []{ "1", "2", "3" };
Arr.Shift(ref data);
// "1"
// data.Length : 2
```

## Unshift

Add a new element at the beginning of the array.

```csharp
var data = new []{ "2", "3" };
var count = Arr.Unshift(ref data, "0", "1");
// count : 4
// data ["0", "1", "2", "3"]
```

## Reverse

Return arrays in reverse order.

```csharp
Arr.Reverse(new []{ "1", "2", "3" });
// result : ["3", "2", "1"]
```

## IndexOf

Retrieve the initial element index that matches all matching values from the array. If it returns -1, it means that it does not appear.

```csharp
Arr.IndexOf(new []{ "1", "2", "3", "4", "5" } , new []{ "2", "3" });
// 1
```

## IndexOfAny

Retrieve the subscript of the specified arbitrary matching value from the array. If it returns -1, it means that it does not appear.

```csharp
Arr.IndexOfAny(new []{ "1", "2", "3", "4", "5" } , new []{ "5", "2" });
// 1
```

## Difference

Exclude the specified value in the array.

```csharp
Arr.Difference(new []{ "1", "2", "3", "4", "5" } , new []{ "2", "3" });
// result : [ "1", "4", "5" ]
```

## RemoveAt

Remove and return the array element of the specified index.

If the index is passed a negative number then it will be removed from the end.

```csharp
var src = new object[]{ "1", "2", "3" };
Arr.RemoveAt(ref src, 1); 
// 2
// src : object[] { "1", "3" }
```

## Cut

Crop the array to the desired position.

If the parameter is negative then it will be cropped from the tail.

```csharp
var data = new char[] { '1', '2', '3', '4', '5' };
Arr.Cut(ref data, 1);
// data[0] == '2'
```

```csharp
var data = new char[] { '1', '2', '3', '4', '5' };
Arr.Cut(ref data, -2);
// data[0] == '1'
// data[1] == '2'
// data[2] == '3'
// data.length = 3
```

## Test

Pass the specified array to the callback test.

The function returns false only if all elements pass the checker are false.

```csharp
var data = new string[]{ "1", "2", "3" };
Arr.Test(data, (value)=> value == "2"); 
// true
```

## Set

Finds the specified element in the specified array, and replaces it with a substitute value if found, otherwise adds a replacement value at the end of the specified array.

```csharp
var data = new[] { "a", "ab", "abc", "abcd", "abcdef" };
Arr.Set(ref data, (element) => element == "none", "hello");
// { "a", "ab", "abc", "abcd", "abcdef", "hello" }

Arr.Set(ref data, (element) => element == "abc", "hello");
// { "a", "ab", "hello", "abcd", "abcdef" }
```