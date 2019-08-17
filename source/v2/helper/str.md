---
title: Str
---

# Str

Str helper extends some common operations for string.

## Method

Extract the legal function name expressed by the string.

```csharp
Str.Method("Function");                       // Function
Str.Method("ClassName.Function");             // Function
Str.Method("ClassName.Function@NewFunction"); // NewFunction
Str.Method("ClassName.Function@#$%^^&*");     // Function
```

## Is

Determines whether the specified string matches the matching expression. Matching expressions only allow the use of the asterisk `*` wildcard (ie, remove all functions except the asterisk in the regular expression)

```csharp
Str.Is("fo*ar" , "foobar"); 
// true
```

## AsteriskWildcard

Translates the specified string into an asterisk match expression. (ie, remove all functions except the asterisk in the regular expression)

```csharp
Str.AsteriskWildcard("[foo]b*r");
// "\[foo\]b*r"
```

## Split

Splits the string into an array based on length, and the last element may be less than the required length.

```csharp
Str.Split("foobara" , 2);
// ["fo", "ob", "ar", "a"]
```

## Repeat

将字符串重复指定的次数。Repeat the specified number of times the string.

```csharp
Str.Repeat("foo" , 3);
// foofoofoo
```

## Shuffle

Shuffle all characters in the string.

```csharp
Str.Shuffle("foobar");
```

## SubstringCount

Calculate the number of times a substring appears in a string.

This function does not count overlapping substrings.

```csharp
Str.SubstringCount("abcabcab","abcab");
// result : 1
```

## Reverse

Reverse specified string.

```csharp 
Str.Reverse("foobar");
// raboof
```

## Pad

Fill the string with the new length.

If the fill length is less than the original length of the string, no action is taken.

Fill both sides of the string. If it is not even, the right side gets extra padding.

```csharp
Str.Pad("foo", 10, "bar", PadTypes.Both);
// barfoobarb
```

## After

Finds the specified value in the string and returns the rest.

If not found, return the specified string itself.

```csharp
Str.After("foobar","oo");
// bar
```

## Contains

Determine whether the specified string contains the specified substring. Substrings are case sensitive.

Match any element if the condition is an array.

```csharp
Str.Contains("helloworld","llo");
// true
```

## Replace

Replace the match in the specified string. This function is case sensitive.

```csharp
Str.Replace(new []{ "oo", "ar" }, "**", "foobar");
// f**b**
```

## ReplaceFirst

Replace the first occurrence of a match in the specified string. This function is case sensitive.

```csharp
Str.ReplaceFirst("foo","***","foobarfoobar");
// ***barfoobar
```

## ReplaceLast

Replaces the first occurrence of a match in the specified string from the back to the front. This function is case sensitive.

```csharp
Str.ReplaceLast("foo","***","foobarfoobar");
// foobar***bar
```

### Random

Generate a random letter (with case), a string of numbers.

```csharp
Str.Random(8);
// A random value maybe：u8Kn1YUz
```

## Trancate

If the length exceeds the given maximum string length, the string is truncated. The last character of the truncated string will be replaced with the mission string.

```csharp
Str.Trancate("foobarfoobar", 6);
// foo...
```

## JoinList

Returns all sequential combination of the given array.

```csharp
Str.JoinList(new [] { "hello", "world" }, "/"); 
// [0] == "hello";
// [1] == "hello/world";
```

## Levenshtein

Calculate Levenshtein distance between two strings.

> [https://en.wikipedia.org/wiki/Levenshtein_distance](https://en.wikipedia.org/wiki/Levenshtein_distance)

```csharp
Str.Levenshtein("hello", "world"); // 4
Str.Levenshtein("hello", "catlib"); // 5
Str.Levenshtein("hello", "catlib-world"); // 10
```

## Space

Represents an empty string with spaces.

```csharp
Str.Space; 
// " "
```