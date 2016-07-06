# Syntax In Functions

A big part of learning a language is figuring out how to write sentences in a way that other people can understand. Programming languages are the same way, except we need to be even more careful about adhering to the rules of the language. If I ask you "Where's the dog at?" you are smart enough to figure out that I really meant "Where is the dog?" and we can continue our conversation.

Programming languages are not that smart, so we need to learn how to structure our programs in such a way that they can be understood.

## Part 1 : Pattern Matching

The first syntactic construct we will look at is called __pattern matching__. Here's the idea: if we want a function to do different things depending on what its input looks like, all we have to do is specify how it will behave on different kinds or "patterns" of input.

Here is a simple example from the book:

```
lucky :: (Integral a) => a -> String
lucky 7 = "LUCKY NUMBER SEVEN!"
lucky x = "Sorry, you're out of luck pal!"
```

Let's run through what this is doing.
 
* The first line is the type declaration, which says that `lucky` takes an `Integral` value as an input and outputs a `String`. 
* The second line says that if the input looks like `7`, the output will be `"LUCKY NUMBER SEVEN!"`. The next line won't matter at all -- we take the first thing we can get.
* The last line says that if the input is `x` (which could be anything, since we don't use that variable anywhere else in this function), the output will be `"Sorry, you're out of luck pal!"`. The pattern `x` acts as a catch-all here -- `x` is a totally generic pattern that can match any input.

OK, so what happens if we call `lucky 7` in real life? Well, `7` is an integer, so the types check out on line 1. On line two Haskell checks to see if the input `7` looks like the pattern `7`. Cool, we have a pattern match! Now Haskell looks at the second part of line two, the bit after the equals sign. We get that bit as our output. Here's what it looks like in ghci:

```
*Main> lucky 7
"LUCKY NUMBER SEVEN!"

*> Main lucky 99
"Sorry, you're out of luck, pal!"
```

### Task (1a)
Write a function called `snape :: String -> String` that does the following:

* If the input is `"Harry"`, output `"Shut up, Potter"`
* If the input is `"Ron"`, output `"Shut up, Weasley"`
* If the input is `"Hermione"`, output `"Shut up, Granger"`
* Otherwise, output `"Shut up"`

-----

One common use of pattern matching is working with lists. You may remember that Haskell lets us use the `Cons` operator (otherwise known as `(:)`) to add as element to the front of a list. In fact, `[1,2,3]` is just a prettier way of writing `1 : (2 : (3 : []))` or `1 : [2,3]`. 

Take a look at this pattern: `x:xs`. What sort of lists follow this pattern? That is, what sort of lists can we write as some element `x` tacked onto the front of some other list `xs`

Well, `[1,2,3]` does, because we can write it like `1 : [2,3]`. In fact, any list with at least one element follows this pattern, because we can make `x` its first element and `xs` everything after the first element.

By the same logic, the pattern `x:y:rest` can match any list with at least two elements. `x` will match with the first element, `y` will match with the second, and `rest` will match with the rest of the list. You can even write a pattern that will match with any list with at least 100 elements if you really want to.

### Task (1b)
If you define 

```
pickElement :: [a] -> a
pickElement (x:y:z:rest) = z
```

What will happen if you call the following?

* `pickElement [1,2,3,4,5]`?
* `pickElement ["Moony", "Wormtail", "Padfoot", "Prongs"]`
* `pickElement [1..]` (this is the infinite list of positive integers).

Try to figure it out, then try it for yourself.

-----

Note that if we call `pickElement	` on an empty list, we get in trouble:

```
*Main> pickElement []
*** Exception: <interactive>:6:5-32: Non-exhaustive patterns in function
	 pickElement 
```

This happens because the empty list doesn't match the pattern we used the define the function. The patterns were "non-exhaustive" because we didn't tell the function what to do with an input with fewer than 3 elements.

## Part 2: Guards
Suppose you wanted to write a function `chooseRestaurant :: Int -> String` that would take the Yelp rating of the restaurant as an input and output a string detailing how excited you are to eat there, according to these guidelines:

* If the rating is 5, output `"Super-duper excited!!!"`
* If the rating is 4, output `"Somewhat hyped."`
* Otherwise, output `"No. Please no."`

You could do this with pattern-matching (assuming that ratings only go from 1 to 5)

```
chooseRestaurant :: Int -> String
chooseRestaurant 5 = "Super-duper excited!!!"
chooseRestaurant 4 = "Somewhat hyped."
chooseRestaurant 3 = "No. Please no."
chooseRestaurant 2 = "No. Please no."
chooseRestaurant 1 = "No. Please no."
```

Great, problem solved. But imagine if ratings could go from 1 to 100 or something. Then we'd have to type out 100 lines, each with a different pattern. In the words of numerous infomercials, __"There HAS to be a better way!"__

There is, and it's called a Guard. Rather than looking for patterns in our input, we can decide what to output based on properties of our input. For example, the property of being less than or equal to three, in the example above.

Here's the Guard version of `chooseRestaurant`:

```
chooseRestaurant :: Int -> String
chooseRestaurant rating
	| rating <= 3        = "No. Please no."
	| rating == 4        = "Somewhat hyped"
	| rating == 5        = "Super-duper excited!!!"
```

Nice, much better.

### Task (2a)
Using guards, write a function called `comicBookGuy :: Int -> String` that takes a movie rating to the phrase Comic Book Guy (from The Simpsons -- please someone get this) probably said when he first saw it. That is, if the input is `5` the output should be `"Best. Movie. Ever."` and if the input is less than `5` the output should be `"Worst. Movie. Ever."`

-----

One useful keyword is `otherwise`, which simply catches any input that doesn't meet the criteria for the guards that come before it. For example, if we were worried that restaurants could have ratings above `5` on Yelp and wanted to output `"No, that rating is suspicious"` for those restaurants, we could have written `chooseRestaurant` like this:

```
chooseRestaurant :: Int -> String
chooseRestaurant rating
	| rating <= 3        = "No. Please no."
	| rating == 4        = "Somewhat hyped"
	| rating == 5        = "Super-duper excited!!!"
	| otherwise          = "No, that rating is suspicious."
```

### Task (2b)
Using `otherwise`, change the definition of `comicBookGuy` to output `"OMG that movie was amazing. omgomgomg"` if the rating is not less than five or equal to 5.

## Part 3 : Where

Let's write a simple function that will tell us if the square of an integer is less than 10, between 10 and 19, between 20 and 29, or larger than 29.

```
squareSize :: Int -> String
squareSize n
	| n*n < 10       = "Less than ten"
	| n*n < 20       = "Between ten and nineteen"
	| n*n < 30       = "Between twenty and twenty-nine"
	| otherwise      = "Even larger than twenty-nine"
```

OK, that will work, but we had to type `n*n` four times: three times in the function and another to complain about it afterwards. So-called "where bindings" let us avoid all this:

```
squareSize :: Int -> String
squareSize n
	| square < 10       = "Less than ten"
	| square < 20       = "Between ten and nineteen"
	| square < 30       = "Between twenty and twenty-nine"
	| otherwise      = "Even larger than twenty-nine"
	where square = n*n
```

The only difference is that we added a `where` at the end, creating a value called `square`. Then we replaced all the `n*n`s in our original function with `square`, and got a much more readable function.

### Task (3a)
Write a function called `quintSize` that does the same thing as `squareSize`, but using the fifth power of its input rather than the square. 
