---
title: "How to Tdd"
date: 2021-09-25T12:39:13+01:00
tags: ["software design", "tdd"]
draft: false
---

TDD is a process that helps us develop software in short cycles. The idea is to write tests, create the implementation as fast as possible, and improve it. This process follows the following three stages:

1. Red
2. Green
3. Refactor

I will provide an example of implementing the three stages using the `FizzBuzz` kata.

The `Red` stage involves writing a test that fails. Initially, this will be code that does not compile. I found this very confusing because the IDE tells us that everything is wrong. Take this initial test, for example:

`=> fizz_buzz_test.go`

```go
package fizz_buzz_test

import "testing"

func Test(t *testing.T) {
  got := fizzBuzz.Convert(1)
  if got != "1" {
    t.Errorf("got %s, expected 1", got)
  }

  got = fizzBuzz.Convert(3)
  if got != "Fizz" {
    t.Errorf("got %s, expected Fizz", got)
  }

  got = fizzBuzz.Convert(5)
  if got != "Buzz" {
    t.Errorf("got %s, expected Buzz", got)
  }

  got = fizzBuzz.Convert(15)
  if got != "FizzBuzz" {
    t.Errorf("got %s, expected FizzBuzz", got)
  }
}
```

The `fizzBuzz` package and the `Convert()` function do not exist yet. I am using the test to drive the implementation. This approach was weird to me, but over time I got used to it. After some practice, I started perceiving the `Red` stage as an opportunity to plan what to do next. It is also helpful in improving the design of the code. At a very early stage, you have a chance to create a structure for your packages, structs, classes, methods or functions, or whatever terminology you decide to use for the language of choice. You can choose their names and see how they might interact with each other before even being created.

There are a few things that are wrong with the above code. Let us start with the test code itself and the function name `Test()`. It is ambiguous. What is the purpose of the test? Even if I change the function name, the test makes four different assertions. We should start with a test that is named meaningfully and tests only one simple thing:

`=> fizz_buzz_test.go`

```go
package fizz_buzz_test

import (
  "testing"
)

func TestFizzBuzzShould(t *testing.T) {
  t.Run("return 1 when given 1", func(t *testing.T) {
    got := fizzBuzz.Convert(1)
    expect := "1"

    if got != expect {
      t.Errorf("got %s, expected %s", got, expect)
    }
  })
}
```

As you have probably noted, the naming is a bit different. Having `Should` in the test function name and a description for each test helps us create a fluent structure for ourselves when reading the tests. Have a look at Kent Beck’s `4 Rules of Simple Design`. It will give you a bit more insight on naming tests, structuring them, and what they should contain.

Now that we have the first simple test, we need to go to the `Green` stage as fast as possible. First, make sure that the test runs and can fail. Here is a simple implementation that would allow running and failing the test:

`=> fizz_buzz.go`

```go
package fizz_buzz

func Convert(num int) string {
  return ""
}
```

Second, fix the implementation to go `Green`. Here is a simple solution:

`=> fizz_buzz.go`

```go
package fizz_buzz

func Convert(num int) string {
  return "1"
}
```

At first, this might look silly, but the idea is to write just enough code to pass the test to start refactoring quickly. Now we are in the `Refactoring` stage. I do not think it is time to refactor. More code is needed to find patterns. I will add another test:

`=> fizz_buzz_test.go`

```go
package fizz_buzz_test

import (
  fizzBuzz "fizz-buzz"
  "testing"
)

func TestFizzBuzzShould(t *testing.T) {
  t.Run("return 1 when give 1", func(t *testing.T) {
    got := fizzBuzz.Convert(1)
    expect := "1"

    if got != expect {
      t.Errorf("got %s, expected %s", got, expect)
    }
  })

  t.Run("return 2 when given 2", func(t *testing.T) {
    got := fizzBuzz.Convert(2)
    expect := "2"

    if got != expect {
      t.Errorf("got %s, expected %s", got, expect)
    }
  })
}
```

The `Convert()` function implementation is not enough to pass both tests. We are back in `Red`. So, let us run the tests, the new test will fail, and we will do the simplest thing possible to go `Green`:

`=> fizz_buzz.go`

```go
package fizz_buzz

func Convert(num int) string {
  if num == 2 {
    return "2"
  }

  return "1"
}
```

Let us continue doing that until we reach the final solution. Let us be disciplined and follow the process methodically. Here is the final state of the tests:

`=> fizz_buzz_test.go`

```go
package fizz_buzz_test

import (
  fizzBuzz "fizz-buzz"
  "testing"
)

func TestFizzBuzzShould(t *testing.T) {
  testCases := []struct {
    testName string
    number   int
    expect   string
  }{
    {"return 1 when given 1", 1, "1"},
    {"return 2 when given 2", 2, "2"},
    {"return 4 when given 4", 4, "4"},
    {"return Fizz when given 3", 3, "Fizz"},
    {"return Fizz when given 6", 6, "Fizz"},
    {"return Fizz when given 9", 9, "Fizz"},
    {"return Buzz when given 5", 5, "Buzz"},
    {"return Buzz when given 10", 10, "Buzz"},
    {"return Buzz when given 20", 20, "Buzz"},
    {"return FizzBuzz when given 15", 15, "FizzBuzz"},
    {"return FizzBuzz when given 30", 30, "FizzBuzz"},
    {"return FizzBuzz when given 45", 45, "FizzBuzz"},
  }

  for _, test := range testCases {
    t.Run(test.testName, func(t *testing.T) {
      got := fizzBuzz.Convert(test.number)

      if got != test.expect {
        t.Errorf("got %s, expected %s", got, test.expect)
      }
    })
  }
}
```

Another good practice to follow is a `Given / When / Then` structure of the tests. It is hard to see it in the above tests, but the `testCases` are the `Given`, the result of the `Convert()` function is the `When`, and the assertion is the `Then`. This structure ensures consistency in all of our tests and makes them more readable.

Here is the implementation code:

`=> fizz_buzz.go`

```go
package fizz_buzz

import (
  "strconv"
)

func Convert(num int) string {
  if isDivisibleBy(number, 15) {
    return "FizzBuzz"
  }

  if isDivisibleBy(number, 3) {
    return "Fizz"
  }

  if isDivisibleBy(number, 5) {
    return "Buzz"
  }

  return strconv.Itoa(number)
}

func isDivisibleBy(number int, factor int) bool {
  return number%factor == 0
}
```

The TDD process looks slow, but it has many benefits. Some of them include: finding patterns in problems that we are not familiar with, creating less coupled code, forcing us to test the implementation code and creating easily testable modules, and creating a more maintainable code design. TDD also helps in creating documentation for the implementation. Having documentation is good because people can understand what the code is doing, and these `people` are often us. Furthermore, if we have tested our code well, writing a new implementation will eventually fail a test, and it will be easier to spot what is wrong. Then we will have to fix our code or write new tests, i.e., we will get in a cycle of creating well documented and more robust code.

In summary:

1. Write a test introducing functions, methods, structs, or classes in your codebase.
2. Created the implementation of the above mentioned.
3. Red - run the test and make sure it fails.
4. Green - write just enough code to pass the test.
5. Refactor if necessary.
6. Repeat all of the above in small steps.
