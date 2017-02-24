# ScratchTDD

Can you TDD in Scratch?

## Overview

In our team, we use a Test-Driven Development (TDD) approach when writing code. I'm a supporter of this approach and its many benefits. I won't go into the benefits here — you can [Google many articles on the subject](https://www.google.co.uk/search?q=test%20driven%20development) (other search providers are available).

[Scratch](https://scratch.mit.edu), from MIT, is essentially a visual programming environment that _"helps young people learn to think creatively, reason systematically, and work collaboratively — essential skills for life in the 21st century"_. There's a growing movement around teaching kids to code, that will hopefully play an important part in solving the skills and diversity shortages our industry has.

One day, the crazy thought popped into my head: _"Can you TDD in Scratch?"_. I did a brief Google, and couldn't find any examples of TDD in Scratch.

So, in my "10% time", I looked into doing some TDD in Scratch. For my first experiment, I implemented the algorithm for the [Roman Numerals Kata](http://codingdojo.org/kata/RomanNumerals/). I cheated a little here, by implementing the algorithm first and then put a test around it afterward (this _isn't_ TDD!).

![](Roman.png)

This at least proved that it is possible to separate my algorithm logic and have tests around it. So next I decided to do some actual TDD in Scratch...

## Basic TDD

Let's go back to basics and start with a proof that we can do TDD in Scratch by writing a test first, and writing some code to make it pass. Remembering the basics of TDD: that we'll follow a _"Red -> Green -> Refactor"_ feedback loop.

![](RedGreenRefactor.png)

For this example, we'll TDD a very simple function that takes a name as an input, and outputs a greeting containing that name. For example:

_"Hello, Ross. Isn't TDD fun?"_

First of all, let's create an empty test. I've created it as a "block", which is analogous to a function or method in other programming/scripting languages.

This sets a global variable `testResult` to "FAIL" and shows it. I've arranged it so it looks a little like the Scratch Cat says the result.

![](EmptyTest.png)

Next let's write the actual failing test. I've set a testInputName of "Ross", an expected greeting of "Hello, Ross. Isn't TDD fun?". It then calls our — for now, empty — `Greeter` block, which takes a `name` as an input parameter.

The test fails, as expected, because we didn't get our expected result.

![](BasicTDDFailingTest.png)

Now that we have a failing test that describes the behaviour we want from our program, we can write some code in `Greeter` to make the test pass...

![](BasicTDDPassingTest.png)

Voilà! We have a passing test that proves that the `Greeter` function gives us the correct greeting.

We could optionally refactor at this stage — that is, change our code without modifying its overall behaviour — to make it tidier, more readable etc, safe in the knowledge that our test will tell us if we have broken it or if it still works as expected.

We could also write some more tests to check that the program works with different names.

But now that we've proven the basic premise of TDD in Scratch, let's move onto a real problem.

## FizzBuzz

### The problem

[FizzBuzz is a common coding kata](http://codingdojo.org/kata/FizzBuzz/). It is usually posed as such:

Write a program that prints the numbers from 1 to 100. But for multiples of three print "Fizz" instead of the number and for the multiples of five print "Buzz". For numbers which are multiples of both three and five print "FizzBuzz".

For example:
```
1
2
Fizz
4
Buzz
Fizz
7
8
Fizz
Buzz
11
Fizz
13
14
FizzBuzz
16
17
Fizz
19
Buzz
[... etc up to 100]
```

Essentially, this boils down to:
- if a number is divisible by 3, say "FIZZ"
- if a number is divisible by 5, say "BUZZ"
- if a number is divisible by 3 _AND_ 5, say "FIZZBUZZ"
- for all other numbers, say the number.

Let's explore the problem using TDD, and just write the function ("block") that decides what to output for each input number.

### Start with some basic tests

Let's start with a simple failing test `testOneIsOne` that tests that for an input of `1`, the output is `1`...

![](FizzBuzz01_FailingTest.png)

Make it pass by simply returning `1` from our `calculateOutputForNumber` block.

![](FizzBuzz02_PassingTest.png)

Simply returning `1` isn't going to work for all numbers for FizzBuzz, so we should start to explore building out our algorithm by picking another test scenario and writing a failing test for it.

Let's write a failing test for the number `2`.

![](FizzBuzz03_FailingFor2.png)

And make it pass...

![](FizzBuzz04_ProblematicPassing2.png)

But, uh oh, there's a problem. I've made it pass by changing the output to `2`. And our `testResult` is saying "PASS". But _really_ `testOneIsOne` is failing. Our second test result for `testTwoIsTwo` is overwriting it.

So, I refactored my test code a bit so that each individual test now adds its result to a list of `testResults`. We then have another block, `verifyAllTestsPass` that checks for any failures and sets our `overallTestResult` to "PASS" if there are none.

![](FizzBuzz05_TestResultsFailing.png)

That's better, it now correctly tells us that the first test is failing, as is the overall suite of tests. So let's fix our code.

We'll just set the input `number` as the `output` for now...

![](FizzBuzz06_TestResultsPassing.png)

Next, let's add a test to check that an input of `3` returns "FIZZ". This will force us to build some more of our algorithm. Here, we simply check if the number is `3` and set the `output` to "FIZZ" if it is, or the input `number` if it is not.

![](FizzBuzz07_TestFizz.png)

We know that's not good enough. All numbers that are divisible by 3 should say "FIZZ". Let's add another test for `6`, and change our code so that when the number is divided by 3, and the remainder is 0, we say "FIZZ". We'll use the [modulo operator](https://en.wikipedia.org/wiki/Modulo_operation) for that — this effectively tells us that the input number is _wholly_ divisible by 3.

![](FizzBuzz08_NextFizz.png)

### Refactor

#### Extract Descriptive Blocks (Functions/Methods)

![](FizzBuzz09_RefactorStep1.png)

#### Table Testing

![](FizzBuzz10_TableTest.png)

### Buzzin'

![](FizzBuzz11_FailingBuzz.png)

![](FizzBuzz12_PassingBuzz.png)

### FizzBuzz!

![](FizzBuzz13_FizzBuzz.png)

![](FizzBuzz14_MoreTests.png)

### Refactoring Safely

![](FizzBuzz15_RefactoringSafely.png)

## Revisiting Roman Numerals

![](Roman02_Tested.png)
