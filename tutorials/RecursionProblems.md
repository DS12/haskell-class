# Recursion Problems
Here is a list of fun recursion problems. Try working through them in order, and feel free to use a function you wrote to solve an early problem to solve a later one. In fact, bonus points if you manage to do this!

If you haven't already, take a look at the [Recursion chapter](https://github.com/DS12/haskell-class/blob/master/tutorials/chapter4.md) of these notes. That chapter explains recursion and walks you through some examples.

Many of the functions in this document are implemented in Haskell already. When this is the case, the function you write will have `my-` added to its name. For example, there is a `length` function in Haskell, so the function you will implement `myLength`.

You should supply type signatures for all of these functions. Doing so will force you to clarify your thinking and will make writing the function body much easier.

## Part 0: `myLength`

### (0a)
Write a function called `myLength` that computes the length of a list. You should  be able to get these results, for example:

```
Prelude> myLength [1,2,3,4,5]
5
Prelude> myLength []
0
Prelude> myLength "cat"
3
Prelude> myLength ["apple", "banana", "pear", "melon"]
4
```

## Part 1: `mySum` and `myProduct`

### (1a)
Write a function `mySum` that will compute the sum of a list of numbers. The sum of an empty list is `0` (why?).

### (1b)
Write a function `myProduct` that will compute the product of a list of numbers. Hint: what is the product of an empty list?

```
Prelude> mySum [1,2,3,4]
10
Prelude> myProduct [1,2,3,4]
24
```

## Part 2: `myMap` and `myFilter`
Here are two super popular functions. `myMap` allows you to do a transformation to every element of a list, and `myFilter` allows you to pick elements out of a list, based on some condition.

### (2a)
Write a function called `myMap` that will take a function and a list and apply the function to every element of the list. The big challenge here will be in figuring out the type signature.

```
Prelude> myMap (\x -> x+1) [1,2,3]
[2,3,4]
Prelude> myMap myLength [[1,2,3,4], [1,2]]
[4,2]
```

### (2b)
Write a function called `myFilter` that will throw out the elements of a list that do not satisfy some condition.

```
Prelude> myFilter (\x -> x > 0) [-1,-2,4,5]
[4,5]
Prelude> myFilter (/= 'a') "abcd"
"bcd"
```

## Part 3

### (3a)
Write a function called `sumSquares` that takes a list of numbers and computes the sum of their squares. Hint: can you reuse functions you've already written?

```
Prelude> sumSquares [1,2,3]
14
Prelude> sumSquares [1.0, 2.5, 4.5]
27.5
```

### (3b)
Write a function `myReverse` that reverses a list.

```
Prelude> myReverse "abcd"
"dcba"
Prelude> myReverse [1,3,5,9999]
[9999,5,3,1]
```
