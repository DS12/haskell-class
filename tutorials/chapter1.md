# Getting Started in Haskell

[Haskell](http://www.haskell.org/haskellwiki/Haskell) is a purely functional language. Unlike Scala, there is no escape hatch--if you want to write imperative code in Haskell, you are forced to use the the IO and ST monads, and/or a higher-level library built atop these data types (technically, Haskell does include some functions, like unsafePerformIO, that allow you to subvert its purity. But these functions are used extremely rarely, as they don't mix well with Haskell's laziness.) This lack of escape hatch has been one reason why many discoveries of how to write functional programs have come from the Haskell community--in Haskell, the question, "how do I express this program using pure functions?" is not merely an intellectual exercise, it is a prerequisite to getting anything done!

Haskell is in many ways a nicer language for functional programming than Scala, and if you are serious about learning more FP, we recommend learning it. We recommend this even if you continue to program predominantly in Scala. 

## Part 0: Environment Setup

First things first. Lets all make sure we have Haskell installed and we have a decent text editor. To get Haskell we will use [Homebrew](http://brew.sh/), which is a package manager for OSX. Check to 


### Task (0a): Install Homebrew

Check to see whether you have Homebrew installed by opening up a Terminal (`/Applications/Utilities/Terminal.app/`) and enter the following code:

`which brew`

If you get back the pathname `/usr/local/bin/brew` then you are good to go. Otherwise you do not have Homebrew installed. Never fear though it's a cinch to set up, just enter the following into your Terminal:

`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

A tutorial should have roughly ten Tasks (a few more or less is ok depending on Task difficulty), labs can have more. Each Task should have some deliverable source code (often in the form of a function implementation). Tasks in Part 0 should generally correspond to project setup or data acquisition.

### Task (0b): Install GHC

With Homebrew installed, getting Haskell is a piece of cake, simply enter the following into your Terminal:

`brew install ghc`

GHC stands for ['Glasgow Haskell Compiler'](https://wiki.haskell.org/GHC). The 'Glasgow' comes from the fact that GHC was originally written by Kevin Hammond in 1989 at the University of Glasgow. A compiler is a computer program (actually a set of programs) that transforms source code written in a programming language (the source language) into another computer language (the target language), with the latter often having a binary form known as object code. The most common reason for converting source code is to create an executable program. GHC is an open source native code compiler for Haskell, and it is [itself written in Haskell](https://ghc.haskell.org/trac/ghc/wiki/Commentary/Compiler). If this sounds weird that's because it is. Compilers are cool.

### Task (0b): Fire up GHCi

One of the programs that comes with GHC is the interpreter GHCi. An interpreter (sometimes called a [REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)) is an interactive computer programming environment that takes inputs, evaluates them, and returns the result in a call-and-reponse fashion very much like a [Unix shell](https://en.wikipedia.org/wiki/Unix_shell) (which is what your Terminal actually is). 

Start GHCi by simply typing `ghci` into your shell. You should see a prompt similar to the one below. 

```
cem3394$ ghci
GHCi, version 7.10.3: http://www.haskell.org/ghc/  :? for help
```

Take the tip and have a look at the help menu:

```
Prelude> :?
 Commands available from the prompt:

   <statement>                 evaluate/run <statement>
   :                           repeat last command
   :{\n ..lines.. \n:}\n       multiline command
   :add [*]<module> ...        add module(s) to the current target set
   :browse[!] [[*]<mod>]       display the names defined by module <mod>
                               (!: more details; *: all top-level names)
   :cd <dir>                   change directory to <dir>
   :cmd <expr>                 run the commands returned by <expr>::IO String
   :complete <dom> [<rng>] <s> list completions for partial input string
   :ctags[!] [<file>]          create tags file for Vi (default: "tags")
                               (!: use regex instead of line number)
   :def <cmd> <expr>           define command :<cmd> (later defined command has
                               precedence, ::<cmd> is always a builtin command)
   :edit <file>                edit file
   :edit                       edit last module
   :etags [<file>]             create tags file for Emacs (default: "TAGS")
   :help, :?                   display this list of commands
   :info[!] [<name> ...]       display information about the given names
                               (!: do not filter instances)
   :issafe [<mod>]             display safe haskell information of module <mod>
   :kind[!] <type>             show the kind of <type>
                               (!: also print the normalised type)
   :load [*]<module> ...       load module(s) and their dependents
   :main [<arguments> ...]     run the main function with the given arguments
   :module [+/-] [*]<mod> ...  set the context for expression evaluation
   :quit                       exit GHCi
   :reload                     reload the current module set
   :run function [<arguments> ...] run the function with the given arguments
   :script <filename>          run the script <filename>
   :type <expr>                show the type of <expr>
   :undef <cmd>                undefine user-defined command :<cmd>
   :!<command>                 run the shell command <command>

 -- Commands for debugging:

   :abandon                    at a breakpoint, abandon current computation
   :back                       go back in the history (after :trace)
   :break [<mod>] <l> [<col>]  set a breakpoint at the specified location
   :break <name>               set a breakpoint on the specified function
   :continue                   resume after a breakpoint
   :delete <number>            delete the specified breakpoint
   :delete *                   delete all breakpoints
   :force <expr>               print <expr>, forcing unevaluated parts
   :forward                    go forward in the history (after :back)
   :history [<n>]              after :trace, show the execution history
   :list                       show the source code around current breakpoint
   :list <identifier>          show the source code for <identifier>
   :list [<module>] <line>     show the source code around line number <line>
   :print [<name> ...]         show a value without forcing its computation
   :sprint [<name> ...]        simplified version of :print
   :step                       single-step after stopping at a breakpoint
   :step <expr>                single-step into <expr>
   :steplocal                  single-step within the current top-level binding
   :stepmodule                 single-step restricted to the current module
   :trace                      trace after stopping at a breakpoint
   :trace <expr>               evaluate <expr> with tracing on (see :history)

 -- Commands for changing settings:

   :set <option> ...           set options
   :seti <option> ...          set options for interactive evaluation only
   :set args <arg> ...         set the arguments returned by System.getArgs
   :set prog <progname>        set the value returned by System.getProgName
   :set prompt <prompt>        set the prompt used in GHCi
   :set prompt2 <prompt>       set the continuation prompt used in GHCi
   :set editor <cmd>           set the command used for :edit
   :set stop [<n>] <cmd>       set the command to run when a breakpoint is hit
   :unset <option> ...         unset options

  Options for ':set' and ':unset':

    +m            allow multiline commands
    +r            revert top-level expressions after each evaluation
    +s            print timing/memory stats after each evaluation
    +t            print type after evaluation
    -<flags>      most GHC command line flags can also be set here
                         (eg. -v2, -XFlexibleInstances, etc.)
                    for GHCi-specific flags, see User's Guide,
                    Flag reference, Interactive-mode options

 -- Commands for displaying information:

   :show bindings              show the current bindings made at the prompt
   :show breaks                show the active breakpoints
   :show context               show the breakpoint context
   :show imports               show the current imports
   :show linker                show current linker state
   :show modules               show the currently loaded modules
   :show packages              show the currently active package flags
   :show paths                 show the currently active search paths
   :show language              show the currently active language flags
   :show <setting>             show value of <setting>, which is one of
                                  [args, prog, prompt, editor, stop]
   :showi language             show language flags for interactive evaluation
```

That's a lot of stuff! Don't worry we won't need most of it anytime soon. One useful thing to know is that many of these commands can be run just with their first letter (e.g. `:q` instead of `:quit`). 

For now lets set multi-line mode (`:set +m`). This lets us define multi-line functions in the REPL, which will be useful in a minute.

### Task (0c): Install Sublime

We're almost ready to rock. The last thing we need is a decent text editor. A text editor is a program for editing source code. Text editors are not the same as word processing environments like Word or Pages, which add lots of extra formatting data to text that compilers like GHC don't understand. 

Picking a text editor is kind of like picking a car. There are lightweight agile ones like emacs or vim, and there are heavy feature-rich ones like Eclipse. For now let's go with [Sublime](https://www.sublimetext.com/), which is sort of like the Hyundai Elantra of text editors. Later when you are a Haskell ninja you can move over to [Yi](http://yi-editor.github.io/pages/installing/).

---

## Part 1: Writing Haskell Programs

Let's keep it old skool and start by creating a simple `Hello world!` program. 

### Task (1a): Hello World in Haskell

Open up a new file in Sublime and paste in the following code:

``` Haskell
module Main where   -- A comment, starting with `--` 

-- Equivalent to `def main: IO[Unit] = ...` in Scala
main :: IO ()
main = putStrLn "Hello world!!"
```

Then save the file with the name `Main.hs`. 

Already we can see some interesting language elements. Module declarations start with the keyword `module`, then are followed by a module name (here `Main`, but `Foo.Bar.Baz` and `Data.List` are also valid module names), then the keyword `where`. Like Java, module names must follow directory structure, so `Foo.Bar` must live in a file called `Bar.hs` inside the `Foo/` directory.
  
The entry point of a Haskell program is the `main` function. In Haskell, type signatures _precede_ the declaration of the value. So `main :: IO ()` is a type signature, preceding the definition of `main = putStr "Hello world!!"` on the subsequent line. We pronounce the `::` symbol as 'has type'.

Since Haskell is pure, it uses an `IO` type to represent computations that may interact with the outside world. Unlike Scala, `IO` is built into the language--you do not need to write your own `IO` type like we did in chapter 13. And there are a couple other differences in how we write type signatures--the `Unit` type from Scala is written as `()` in Haskell. (Like Scala, it has a single value, written `()`) And _type application_ uses juxtaposition, rather than square brackets. For comparison:

* `IO[Unit]` (Scala) vs `IO ()` (Haskell)
* `Map[String,Double]` vs `Map String Double`
* `Map[String,Map[K,V]]` vs `Map String (Map k v)`: In Haskell, type variables must begin with a lowercase letter. Names starting with uppercase are reserved for concrete types and data constructor names. 

### Task (1b): Compilation

We can compile `Main.hs` using GHC from a new Terminal window like so: 

```
cem3394$ ghc --make Main
[1 of 1] Compiling Main             ( Main.hs, Main.o )
Linking Main ...
```

First however you're going to have to make sure you're in the right directory. Ask your neighbor or teach yourself the `pwd`, `cd` and `ls` [commands in Unix](http://mally.stanford.edu/~sr/computing/basic-unix.html). 

What is linking? Ah, I was hoping you'd ask! Here's another Wikipedia [rabbit hole](https://en.wikipedia.org/wiki/Linker_(computing)) for you.

### Task (1c): Execution

Now that your `Main.hs` code is compiled, try and run it as an executable in your Terminal:

```
cem3394$ ./Main
Hello world!!
```

You should also try starting a REPL and loading your `main` fuction in this way:

```
cem3394$ ghci
GHCi, version 7.10.3: http://www.haskell.org/ghc/  :? for help
Prelude> :l Main
[1 of 1] Compiling Main             ( Main.hs, interpreted )
Ok, modules loaded: Main.
*Main> main
Hello world!!
*Main>
```

Congrats!! You've written and compiled your first Haskell program! You are now a Haskell progammer! In classic LinkedIn style you should put this skill on your page immediately and ask your neighbors for endorsements. Watch the recruiting emails come rolling in. 

---

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

A really good tool for finding haskell functions is [Hoogle](https://www.haskell.org/hoogle/), which allows you to search by type signature among other things. Get the type of `foo` and enter it into Hoogle. What other functions have that same type?

### Ranges

A super handy way to make a list is using ranges:

```
Prelude> [1..10]
[1,2,3,4,5,6,7,8,9,10]
Prelude> ['a' .. 'z']
"abcdefghijklmnopqrstuvwxyz"
```

You can also specify steps:

```
Prelude> [1,3 .. 10]
[1,3,5,7,9]
```

### List Comprehensions

The pattern of chaining operations is so useful and is used for lots of different things in Haskell that it has its own operator `>>=` (called bind). It is equivalent to `flatMap` in Scala.

```Haskell
[1..10] >>= (\x -> if odd x then [x*2] else [])
```

### Task (3c): Repeat the odd numbers in a List

Use the bind operator and a lambda to repeat the odd numbers in a List. So the list `[1..10]` should generate the list `[1,1,2,3,3,4,5,5,6,7,7,8,9,9,10]`
 
The examples above use things called monads, which are like computation builders. The common theme is that the monad chains operations in some specific, useful way. In the list comprehension, the operations are chained such that if an operation returns a list, then the following operations are performed on every item in the list.

### Task (3c): Functional IO

Run the following line of code in your REPL:

```Haskell
putStrLn "What is your name?" >>= (\_ -> getLine) >>= (\name -> putStrLn ("Welcome, " ++ name ++ "!"))
```
What happens? 

This is an example of the bind operator for the IO monad. The IO monad performs the operations sequentially, but passes a "hidden variable" along, which represents "the state of the world", which allows us to write I/O code in a pure functional manner.




---

## Resources

Include other relevant references (blog/SO posts, articles, books, etc) here.