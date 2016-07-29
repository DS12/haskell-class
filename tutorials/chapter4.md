# Chapter 4: Recursion

## Part 1
In this tutorial we will talk about one of the most important concepts in functional programming: recursion. A function is _recursive_ if it defined in terms of itself. Here is a recursive function.

```
loop x = loop x
```

Obviously if we run this it will never actually do anything; it will just keep on calling itself forever. To avoid this, we often specify a _base case_ where the function does not call itself but instead takes some concrete value. In general, the base case should be the simplest possible input, and every time the function calls itself it should move closer to the base case.

Here is an example using the Fibonacci sequence, which is one of the most famous sequences in mathematics. Even mathematically, it is defined recursively (there are non-recursive definitions as well but they are more complicated) as follows. The first two Fibonacci numbers are 0 and 1. From then on, each Fibonacci number is the sum of the previous two.

So the sequence goes 0,1,1,2,3,5,8,13,...

```
fib :: Int -> Int
fib 0 = 0
fib 1 = 1 
fib n = fib (n-1) + fib (n-2)
```

Let's go through this function. The first line is the type declaration, which says that `fib` will take an integer as its input and spit one out as its output. The second and third lines are the base cases -- they tell `fib` what to do on the simplest and most basic inputs. The last line is the interesting one. It says that if we feed `fib` some value `n` that is not `0` or `1`, then it will take the two preceding Fibonacci numbers and add them up.

### (1a)
What will you get if you call `fib 5`? Try to work it out in your head or on paper before running the code.

### (1b)
What will you get if you call `fib (-2)`? Again, try to reason it out before running anything.

---

One of the great things about recursion is that we can solve problems by answering two questions:

* What is the simplest case of this problem?
* How can we reduce a complex version of this problem to a simpler version?

With Fibonacci, these questions were answered for us in the definition of the sequence. The simplest Fibonacci numbers to find are the first two, because we are told exactly what they are. To reduce the problem of finding `fib n`, we just have to look at `fib (n-1)` and `fib (n-2)`.

## Part 2

Let's write a function that will take a list of integers and pull out the biggest one. For simplicity, let's say that the maximum of an empty list is `minBound`.

```
biggest :: [Int] -> Int
```

### (2a)
What is the simplest case of this problem?

---

How do we simplify a complex case of the "biggest-finding" problem? With lists the first thing you should do is view the first element (or _head_) of the list as "special" and the rest of the list (or _tail_) as a "less complex" list to worry about.

__Fact 1__: If the first element is bigger than the biggest element from the rest of the list, then our answer will just be the first element.

__Fact 2__: If the first element is not the biggest, then our answer will be the biggest element from the rest of the list.

Now all we have to do is convert the statements above into code. Here's how we do it.

```
biggest :: [Int] -> Int
biggest [] = minBound
biggest (x:xs)
    | x > (biggest xs)  = x
    | otherwise  = (biggest xs)
```

OK, the first line is the old type declaration. The second is the base case, since we defined the biggest element from an empty list to be `minBound` for this exercise. The third line is a pattern match. If the input isn't empty, we break it down in terms of its first element and its tail.

The last two lines are just Fact 1 and Fact 2 translated into code.

Note that we could also have used a `where` binding to avoid writing `biggest xs)` multiple times.

```
biggest :: [Int] -> Int
biggest [] = minBound
biggest (x:xs)
    | x > biggestFromTail  = x
    | otherwise  = biggestFromTail
    where biggestFromTail = biggest xs
```

In fact, `biggest` is already implemented for you in Haskell, though it is called `maximum`.

### (2b)
Write a function `smallest :: [Int] -> Int` that computes the smallest number from a list. Let's define the smallest number in the empty list to be `maxBound`.

### (2c)
Write a function `contains :: (Eq a) => a -> [a] -> Bool` that tests if an element lies in a list. For example:

```
Prelude> contains 2 [1,2,3]
True
Prelude> contains 2 [1,3,4,5]
False
Prelude> contains 'q' "Haskell is great"
False
```


## Part 3: Sorting
Often you will have a list of things and will want to put them in order. This is called _sorting_. There are a variety of sorting algorithms, but we will discuss one in particular. Before going any further, let's remember an extremely useful bit of Haskell syntax: the _list comprehension_.

Suppose I have a list `myList = [1,2,3,4,5]`, and I want to double its members, except for `5`. There are many ways of doing this, but one way is to tell Haskell exactly what you want, what you really really want.

```
Prelude> [2*x | x <- myList, x /= 5]
[2,4,6,8,10]
```

All this means is "for every member `x` of `myList` that is not equal to `5`, give me `2*x`".

Cool, so now we need to figure out how to sort a list. Thinking back to Part 1, let's try to answer these questions:

* What is the simplest case of this problem?
* How can we reduce a complex version of this problem to a simpler version?

Hm, well the simplest list to sort is definitely the empty list, because there is no sorting to be done. The same goes for a list with only one element.

The answer to the second question is actually pretty clever. Suppose we had the list `listToSort = [3,2,6,1,4,5]` and we wanted to put it in ascending order. As always, we'll match the pattern `(x:xs)`, so `x=3` and `xs = [2,6,1,4,5]`. 

The final answer will be `[1,2,3,4,5,6]`. Obviously the elements to the left of `3` are the ones that are less than `3` and the elements to right of `3` are the ones that are greater than `3`. So the problem of sorting `[3,2,6,1,4,5]` can be broken down into two simpler sorting problems.

* Sort the stuff that is smaller than `3`
* Sort the stuff that is bigger than `3`

Then we take the former and stitch the latter to the end, with a `3` in the middle.

### (3a)
Implement a function `sort :: (Ord a) => [a] -> [a]` that does this.




