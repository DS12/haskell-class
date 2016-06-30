# Getting Started in Haskell

## Part 1: Expressions

## Part 2: Functions

### Anonymous Functions

Function literal syntax is a bit different in Haskell than in Scala. In Scala, we'd write `(h,t) => ...`, in Haskell, we write `\h t -> ...`. The parameters to a lambda function mirrors the syntax for named function application (see below), with parameters separated by spaces. Following the parameter list is the `->`, then the body of the lambda. So, for example:

```
Prelude> (\x -> 2*x) 10
20
```

I can also get type information about anonymous functions (indeed about pretty much anything in Haskell), by using the `:type` or `:t` command like so:

```
Prelude> :t (\x -> 2*x) 
(\x -> 2*x) :: Num a => a -> a
```

Note that function types are written using `->` instead of `=>` as in Scala. 



### Task (2a): Functions Eating Functions

In Haskell functions are [first class objects](https://en.wikipedia.org/wiki/First-class_function), which means we can pass functions to functions just as easily as we can pass other data types. Make a lambda function that eats another function and applies it to the value 2, then feed it the lambda `(\y -> y + 2)`. What result do you get?

Now use the `:t` command on your first lambda, what kind of type is it?

This snippet shows a few new things. Let's start by looking at the type signature. What's with all the `->` symbols? Functions in Haskell are curried by default. Although we can write `(Int,Int) -> Int`, for the type of a function that expects a pair of integers and returns an `Int`, we almost always work with curried functions in Haskell and the language has special support for interpreting curried function application efficiently. The `->` operator is right-associative, so for example the type `(a -> b -> b) -> b -> List a -> b` can be read the same way you'd read the Scala signature:

```Scala
def foldRight[A,B](f: (A,B) => B, z: B, xs: List[A]): B
```

Note also that unlike Scala, Haskell does not have a distinction between `val`, `def`, `lazy val`, and `var` (data are immutable and lazily evaluated). All declarations use the same syntax, a symbol name, followed by any arguments or patterns, followed by an `=` sign, followed by the body of the definition. 

### Task (2b): Partial Application

Make a lambda function that adds two numbers together and apply it to the values 5 and 10 as above. Experiment with using the `:t` command on the lambda when applied to both, one, or no arguments respectively. In the last case the resulting type should be `Num a => a -> a -> a`, which is similar but not quite equal to what you had in Task (2b). Any ideas why?


### Task (2c): Googling Errors

If you enter your lambda function from above into GHCi without any arguments, you should get an error that looks something like this:

```
No instance for (Show (a0 -> a0 -> a0)) (maybe you haven't applied enough arguments to a function?)
```

WTF? Never fear, this is an opportunity to participate in the time-honored programming tradition known as 'Googling your error'. When I Googled the first part of that error (without the suggestion), my top hit was the following link:

http://stackoverflow.com/questions/18615666/no-instance-for-show-a0-arising-from-a-use-of-print-the-type-variable-a0-i

That's somewhat useful (& you should definitely make friends with SO, a fun place to start is with the [Zalgo post](http://stackoverflow.com/questions/1732348/regex-match-open-tags-except-xhtml-self-contained-tags)), however the answer is over-specific. We can help ourselves by removing the (rather specific) type information from our error message. Try Googling `No instance for Show` instead. You should get a wider variety of results including [this one](http://users.jyu.fi/~sapekiis/haskell-pitfalls/#no-instance-for-show), which tells us that we need to define the show function from the Show type class. We'll discuss type classes next week.


### Named Functions

Lambdas are great, but at some point you have to start giving functions names. Here are two equivalent ways to do that:

```
inc = \x -> x+1
```
or

```
inc x = x+1
```

The syntax for function application is simple juxtaposition, with arguments separated by spaces, as in `go n 1` (vs `go(n, 1)` in Scala). Function application is left-associative and binds tighter than any other operation, so `f x y+1` would actually be parsed as `(f x y) + 1`, and we can use parentheses to obtain a desired grouping.


```
factorial 0 = 1
factorial n = n * factorial (n-1)
```

This example demonstrates some simple pattern matching in Haskell, matching on the numeric literal `0`. We can write a function's name several times, supplying a pattern for each argument. Patterns are matched top to bottom, as in Scala. We'll talk more about Haskell's pattern matching in the next section.

You can optionally preface a funtion with its type signature like so:

```
factorial :: Int -> Int
factorial 0 = 1
factorial n = n * factorial (n-1)
```

This is not required, but it is considered good form when writing actual programs.

### Task (2d): Functions in the REPL

You can put `factorial` into a file and compile it like we did with our HelloWorld function, but let's try to define it directly in the REPL.

You can define multi-line functions in the REPL by using the `let` keyword and braces like so:

```
let { factorial 0 = 1 ; factorial n = n * factorial (n-1) }
```

Try that and then make sure `factorial 3` gives the result you'd expect (i.e. 6). Now lets try a [second way](http://stackoverflow.com/questions/2846050/how-to-define-a-function-in-ghci-across-multiple-lines):

```
Prelude> :{
Prelude| let factorial 0 = 1
Prelude|     factorial n = n * factorial (n-1)
Prelude| :}
Prelude> factorial 3
6
```

Do this yourself. If you're not careful you may get an error like this: `<interactive>:156:3: parse error on input ‘where’`. That's because, like Python, Haskell is sensitive to white-space. Basically that means that Haskell determines where blocks begin and end based on indentation. Certain keywords (like `let` and `where`) introduce a _layout block_. Check out the [following link](https://en.wikibooks.org/wiki/Haskell/Indentation) to get the hang of how to indent code properly.


### Where and Let 

Let's look at a couple of other definitions for the factorial function: 

```Haskell
factorial' :: Int -> Int
factorial' n = go n 1
  where go 0 acc = acc
        go n acc = go (n-1) (n * acc)

factorial'' :: Int -> Int
factorial'' n = 
  let go 0 acc = acc
      go n acc = go (n-1) (n * acc)
  in go n 1
```

A few things about this code:
* Identifier names in Haskell can contain the `'` symbol (as well as `_`, letters, and numbers, though data types must start with a capital letter and other identifiers cannot start with a number or `'`). 
* We are also making use of a local function to write our loop in both `factorial'` and `factorial''`, much like we'd do in Scala, though unlike Scala, _all tail calls are optimized_. Syntactically, we can place local definition(s) _after_ the expression that references it, using a `where`-clause (used in `factorial'`) or we can place the local definitions _before_ the expression referencing it, using a `let` expression (used in `factorial''`).

### Task (2e): Tail Recursion (Optional)

`factorial` and `factorial'` are both recursive, but only `factorial'` is [tail recursive](https://en.wikipedia.org/wiki/Tail_call). Why?

### Task (2f): Fibonacci

Using the `factorial` code above as a model, write a function `fibonacci` that computes the nth [Fibonacci number](https://en.wikipedia.org/wiki/Fibonacci_number):

```
Prelude> map fibonacci [0..10]
[1,1,2,3,5,8,13,21,34,55,89]
```

## Part 3: Lists and Strings

Lists are probably the most used data structure in Haskell. In this section we'll look at the basics of lists, strings (which are lists) and list comprehensions.

https://wiki.haskell.org/Cookbook/Lists_and_strings

### Lists 

A list is a homogenous data structure. It stores several elements of the same type. That means that we can have a list of integers or a list of characters but we can't have a list that has a few integers and then a few characters. We can write list literals using the syntax `[1,2,3,4]`, `["a","b"]`.

Lists in Haskell come built in and have some special syntax. We write the type `List Int` as `[Int]`, we write `Nil` as `[]`, and `Cons h t` as `h:t`. 

### Task (3a): WTF is Cons?

Anytime you are not sure what something is in Haskell, check out its type:

```
:t (:)
```

Now that you know the two things it eats, try feeding it two of those things and see what it spits out. What does `:` do?


### Task (3a): Heads and Tails

Two more commonly used list functions are `head` and `tail`. Test these out and then combine them to make a function `foo` that gets the second element of a list like so:

```
Prelude> foo [1,2,3,4]
2
Prelude> foo "Yoyoyo"
'o'
```

### Strings

As we just saw, a string is simply a list of characters.

```
Prelude> 'i' : []
"i"
Prelude> 'h' : "i"
"hi"
Prelude> head "They alive, dammit!"
'T'
```

### Task (3b): Hoogle it

A great tool for finding haskell functions is [Hoogle](https://www.haskell.org/hoogle/), which allows you to search by type signature among other things. Get the type of `foo` and enter it into Hoogle. What other functions have that same type?

### Ranges

One handy way to make a list is using ranges:

```
Prelude> [1..10]
[1,2,3,4,5,6,7,8,9,10]
Prelude> ['a' .. 'z']
"abcdefghijklmnopqrstuvwxyz"
```

You can also specify steps implicitly like so:

```
Prelude> [1,3 .. 10]
[1,3,5,7,9]
```

### List Comprehensions

The pattern of chaining operations is so useful in Haskell that it has its own operator `>>=` (called bind). It is equivalent to `flatMap` in Scala.

```Haskell
[1..10] >>= (\x -> if odd x then [x*2] else [])
```

### Task (3c): Repeat the odd numbers in a List

Use the bind operator and a lambda to repeat the odd numbers in a List. So the list `[1..10]` should generate the list `[1,1,2,3,3,4,5,5,6,7,7,8,9,9,10]`
 
The examples above use things called monads, which are like computation builders. The common theme is that the monad chains operations in some specific, useful way. In the list comprehension, the operations are chained such that if an operation returns a list, then the following operations are performed on every item in the list.

### Task (3c): Functional IO

Bind does a number of interesting things in Haskell. Try running the following line of code in your REPL:

```Haskell
putStrLn "What is your name?" >>= (\_ -> getLine) >>= (\name -> putStrLn ("Welcome, " ++ name ++ "!"))
```
What happens? 

This is an example of the bind operator for the IO monad. The IO monad performs the operations sequentially, but passes a "hidden variable" along, which represents "the state of the world", which allows us to write I/O code in a pure functional manner.


### "Infinite" Data Structures

One advantage of the non-strict nature of Haskell is that data constructors are non-strict, too. This should not be surprising, since constructors are really just a special kind of function (the distinguishing feature being that they can be used in pattern matching). For example, the constructor for lists, (:), is non-strict.

Non-strict constructors permit the definition of (conceptually) infinite data structures. Here is an infinite list of ones: 

```
ones = 1 : ones
```

Perhaps more interesting is the function numsFrom: 

numsFrom n = n : numsFrom (n+1)

Thus numsFrom n is the infinite list of successive integers beginning with n. From it we can construct an infinite list of squares: 

```
squares = map (^2) (numsfrom 0)
```

(Note the use of a section; ^ is the infix exponentiation operator.)

Of course, eventually we expect to extract some finite portion of the list for actual computation, and there are lots of predefined functions in Haskell that do this sort of thing: take, takeWhile, filter, and others. The definition of Haskell includes a large set of built-in functions and types---this is called the "Standard Prelude". The complete Standard Prelude is included in Appendix A of the Haskell report; see the portion named PreludeList for many useful functions involving lists. For example, take removes the first n elements from a list:

```
take 5 squares => [0,1,4,9,16]
```

The definition of ones above is an example of a circular list. In most circumstances laziness has an important impact on efficiency, since an implementation can be expected to implement the list as a true circular structure, thus saving space.

For another example of the use of circularity, the Fibonacci sequence can be computed efficiently as the following infinite sequence: 

```
fib = 1 : 1 : [ a+b | (a,b) <- zip fib (tail fib) ]
```

where zip is a Standard Prelude function that returns the pairwise interleaving of its two list arguments: 

```
zip (x:xs) (y:ys) = (x,y) : zip xs ys
zip xs ys = []
```

Note how fib, an infinite list, is defined in terms of itself, as if it were "chasing its tail."

---

## Resources

Include other relevant references (blog/SO posts, articles, books, etc) here.