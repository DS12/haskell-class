# Types and Typeclasses

## Types

As you've seen briefly, there are many different kinds of data we interact with when writing programs. You've already
seen `Integer`s like the numbers 1,2,3,4 ; `String`s like the series of characters `"hello world"` ; as well as lists
like the collection of numbers `[1,2,3,4]`. One common kind is called a `Float` which represents a number with decimals.
For example, 1.2345. These different kinds of data referred to as different "types". By thinking  about the type of our
data, we can catch mistakes before we actually execute our code. And whether we like to admit it
or not, we are always dealing with types (even when the language doesn't make it explicit) because we make implicit
assumptions in our code. For example:

```haskell
div a b = a / b
```

This simple function assumes that it makes sense to divide the first argument by the second. However, that is not
always the case. What do you think would happen if we divided one string by another? Let's make the REPL tell us.
Fire up the console by executing `ghci` in your terminal and execute the following line:

```haskell
Prelude > "hello" / "world"
```

You should see the following error message:

    No instance for (Fractional [Char]) arising from a use of ‘/’
    In the expression: "asdf" / "asdf"
    In an equation for ‘it’: it = "asdf" / "asdf"

As you would have expected, we get an error telling us that we cannot divide one string by another. And even though
we **could** rely on our awesome programming skills to ensure we never pass two strings to our `div` function, it would
be nice if we could get a warning if we ever made such a mistake. Luckily, Haskell (as well as many other languages)
lets us make these assumptions in what is called the "type signature" of a function.

Expressing the type signature of a function takes just a small line above our function definition:

```haskell
div :: Int -> Int -> Float
div a b = a / b
```

The first line is usually read "the function `div` is a takes two `Int`s and returns a `Float`". Question: Why does `div`
have to return a `Float` instead of an `Int`? If you're stumped, think about the answer to `3 / 2`.

A more generic function would probably take two `Float`s instead of `Int`s but you get the idea.

Also, it is helpful to know that you can always check the type signature of a function (and
basically everything else) in the REPL by starting the statement with `:t`:

```
Prelude > :t "Hello"
"hello" :: [Char]
```

### Type variables

Some type signatures are not "concrete". That is to say, the types of certain data structures
are defined in terms of some parameter. For example, since you can have a list of integers
as well as one of strings, the type of a list is denoted by `[a]` where `a` stands for any
type. To illustrate this point, consider `["hello", "world"]`. This element has the type
`[String]` (meaning a list of strings), whereas `[1,2,3]` has the type `[Int]`.

Function type signatures can also be generic in the same sense. Take for example, `head`
which returns the first element in a list. What do you think the type signature would be? Let's
find out!

```haskell
Prelude > :t head
head :: [a] -> a
```

That means that head takes a list of `a` and returns a single `a`.

## Typeclasses


A typeclass is a sort of interface that defines some behavior. If a type is a part of a typeclass, that means that it supports and implements the behavior the typeclass describes. Take the equality function for example:

```
ghci> :t (==)
(==) :: (Eq a) => a -> a -> Bool
```

Everything before the `=>` symbol is called a class constraint. We can read the previous type declaration like this: the equality function takes any two values that are of the same type and returns a Bool. The type of those two values must be a member of the Eq class (this was the class constraint).

The `Eq` typeclass provides an interface for testing for equality. Any type where it makes sense to test for equality between two values of that type should be a member of the Eq class. All standard Haskell types except for `IO` (the type for dealing with input and output) and functions are a part of the `Eq` typeclass.

Typeclasses allow us to generalize over a set of types in order to define and execute a standard set of features for those types. For example, the ability to test values for equality is useful, and we’d want to be able to use that function for data of various types. In fact, we can test any data that has a type that implements the typeclass known as Eq for equality. We do not need separate equality functions for each different type of data; as long as our datatype implements, or instantiates, the Eq typeclass, we can use the standard functions.

Philip Wadler famously described this tension between types and functions as the [Expression Problem](https://en.wikipedia.org/wiki/Expression_problem)

>The expression problem is a new name for an old problem. The goal is to define a datatype by cases, where one can add new cases to the datatype and new functions over the datatype, without recompiling existing code, and while retaining static type safety (e.g., no casts).

Typeclasses and types in Haskell are, in a sense, opposites. Where a declaration of a type defines how that type in
particular is created, a declaration of a typeclass defines how a set of types are consumed or used in computations.
This tension is related to the expression problem which is about defining code in terms of how data is created or
processed.

This chapter explains some important predefined typeclasses, only some of which have to do with numbers, as well as
going into more detail about how typeclasses work more generally.


## Part 1: `Eq`

### Task (1a): Getting type information

Use the GHCi command `:info` to query information about the following types: `Bool`, `Double`, `Int` and `Integer`.

What typeclasses does `Double` implement that `Int` doesn't?

What typeclasses does `Int` implement that `Integer` doesn't?

What does this tell you about the differences between `Double`, `Int` and `Integer` numerically?

The information includes the data declaration for the type in question. In your REPL, it also tells you where the datatype and its instances are defined for the compiler. Next we see a list of instances. Each of these instances is a typeclass that the type in question implements, and the instances are the unique specifications of how the type makes use of the methods from that typeclass.

### Task (1b): Getting typeclass information

Now use `:info` to query information about the following _typeclasses_:`Eq` and `Ord`.

What are the basic functions specified in each typeclass? What might they do?

How does the returned information differ from above?

What is the relationship between types, functions, and typeclasses?

You can use a search engine like [Hoogle](http://haskell.org/hoogle) to find information on Haskell datatypes and typeclasses. This is useful for looking up what the deal is with things like `Fractional`.

Hoogle is a Haskell API search engine, which allows you to search many standard Haskell libraries by function name or type signature. This becomes very useful as you become more fluent in Haskell types as you will be able to input the type of the function you want and find the functions that match.

### Task (1c): `==`

What is the type signature of the `==` function in `Eq`?

What do you think would happen to the `a` if we partially apply it to a string, like so `(==) "cat"`? Test your hypothesis with the `:t` command.

Were you correct? What happened to the `(==) :: Eq a` part? Why?

What would happen if we apply `==` to two cats: `"cat1" == "cat2"`?

What would happen if the first two arguments `a` and `a` to `==` aren’t the same type?

Remember: the type of `a` is usually set by the le most instance of it and can’t change in the signature `Eq a => a -> a -> Bool`. Applying `(==)` to Integer will bind the `a` type variable to `Integer`.

---

## Part 2: Numerical Typeclasses

`Num` is a typeclass implemented by most numeric types. As we see when we query the information, it has a set of predefined functions, like `Eq`:

### Task (2b): `Num` and `Integral`

Use GHCi to look up the functions defined in the `Num` and `Integral` typeclasses.

What do you think the functions in `Num` are for? Use Hoogle to look up any you're unsure of.

`(Real a, Enum a) => Integral a` is a _typeclass constraint_. It means that any type that implements `Integral` must already have instances for Real and `Enum` typeclasses. An integral type must be both a real number and enumerable (more on `Enum` later) and therefore may employ the methods each of those typeclasses. In turn, the `Real` typeclass itself requires an instance of `Num`. So, the Integral typeclass may put the methods of `Real` and `Num` into effect (in addition to those of `Enum`).

Since `Real` cannot override the methods of `Num`, this typeclass inheritance is only additive and the ambiguity problems caused by multiple inheritance in some programming languages — the so-called “deadly diamond of death” — are avoided.

### Task (2b): Tuple Experiment

Look at the types given for `quotRem` and `divMod`. What do you think those functions do? Test your hypotheses by playing with them in the REPL. We’ve given you a sample to start with below:

```
let ones x = snd (divMod x 2)
```

### Task (2c): Parts of a Whole

`quotRem` and `divMod` are actually combined versions of 4 separate functions: `quot` ,`rem`,`div` and `mod`. Let's examine them individually.

What do the following two functions do and why?

```
let id1 x y = (x `quot` y)*y + (x `rem` y)
let id2 x y = (x `div`  y)*y + (x `mod` y)
```
Do certain inputs cause them to break? Why?

One example where the differences would matter is testing if an integer is even or odd.

```
let buggyOdd x = x `rem` 2 == 1
buggyOdd 1 // True
buggyOdd (-1) // False (wrong!)

let odd x = x `mod` 2 == 1
odd 1 // True
odd (-1) // True
```

What is happening here?

### Task (2d): Digging Deeper

At a glance you might think that `quotRem` and `divMod` are the same. Try them on these arguments:

```
divMod (-12) 5
quotRem (-12) 5
```

How do the results differ?

Haskell's `div` follows the convention of mathematicians (or at least number theorists) in always truncating down division (towards negative infinity not 0) so that the remainder is always nonnegative.

### Task (2d): Can Haz Deeper

Now try

```
divMod (-12) 5
divMod 12 (-5)
```

How do the results differ?

Note that both `quotRem` and `divMod` satisfy `(q,r) == divMod x y` if and only if `x == q*y + r`.

---

## Resources

Include other relevant references (blog/SO posts, articles, books, etc) here.
