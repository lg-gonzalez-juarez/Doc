---
title: 09. Hands-on Lab 09
tags: [python]
keywords: sample
#summary: "This is just a sample topic..."
sidebar: python_sidebar
permalink: Lab_09.html
folder: python2
---

# Defining and Using Python Generators

# Introduction

Generators are Python functions that behave like iterators. By using generators we're able to create sequences that are evaluated as each item is needed, making them more memory-efficient compared to simply having lists. In this hands-on lab, we'll be building a generator function that behaves in a way similar to the built-in `range` function, except it will yield string characters instead of integers.

To feel comfortable completing this lab you'll want to know how to do the following:

Defining and using generators. Watch "Defining and Using Generators" video from the Certified Entry-Level Python Programmer Certification course.
Working with for loops. Watch "The for Loop" video from the Certified Entry-Level Python Programmer Certification course.
Using the ord and chr functions. Watch the "String Encodings and Functions" video from the Certified Entry-Level Python Programmer Certification course.

## The Scenario

The `range` function returns an iterable object that makes it incredibly easy to iterate through a large list of integers, without needing to create the list up front. In this hands-on lab, we're going to create the `char_range` generator that will do the same thing, except it will provide us with a range of characters based on Unicode code point for the starting character and ending character, with an optional step value.

Here's our ideal usage in the REPL with a `for` loop and turning the generator into a list:

```
>>> for char in char_range('a', 'e'):
...     print(char)
...
a
b
c
d
e
>>> list(char_range('a', 'e', 2))
['a', 'c', 'e']
>>> list(char_range('e', 'a', 2))
['e', 'c', 'a']
```

Notice that we're able to handle the case where the starting code point is larger than the ending code point. In these situations, we'll need to change the step value to be a negative number. Additionally, unlike the `range` function, we want the result of `char_range` to include the stop character instead of omitting it.

We'll be implementing the char_range function in the `using-generators.py` file, and we can see if our implementation meets the expectations by running the file through the Python interpreter:

```
$ python3.7 using-generators.py
Traceback (most recent call last):
  File "using-generators.py", line 12, in <module>
    char_range
NameError: name 'char_range' is not defined
```

If we run into an expectation that we don't meet, then the error should explain what is expected compared to what is happening.

## Logging In

There are a couple of ways to get in and work with the code. One is to use the credentials provided in the hands-on lab overview page, log in with SSH, and use a text editor in the terminal.

The other is using VS Code in the browser. If you'd like to go this route, then you will need to navigate to the public IP address of the workstation server (provided in the hands-on lab overview page) on port 8080 (example: http://PUBLIC_IP:8080).

## Create the char_range Generator

The first thing that we need to create is the `char_range` function within the `using-generators.py` file. Just creating a function doesn't make it a generator. To to that, we need to use the keyword `yield`. Let's create the function and simply `yield` the start character for the time being:

**using-generators.py** (partial)

```
def char_range(start, stop, step=1):
    yield start
```

Now if we run the file we should make it past the first check that ensures that we're created a generator:

```
$ python3.7 using-generators.py
Traceback (most recent call last):
  File "using-generators.py", line 26, in <module>
    ], f"Expected ['a', 'b', 'c', 'd', 'e'] but got {repr(list(char_range('a', 'e')))}"
AssertionError: Expected ['a', 'b', 'c', 'd', 'e'] but got ['a']
```

Next, we'll need to create a range from our starting character to our ending character by utilizing the Unicode code point for each character. To do this we'll use the `ord` function, and then we'll yield the result of the `chr` function and the code point for each item in our range. Here's how we might write this:

**using-generators.py** (partial)

```
def char_range(start, stop, step=1):
    start_code = ord(start)
    stop_code = ord(stop)

    for value in range(start_code, stop_code, step):
        yield chr(value)
```

Now if we run this we'll see the following error:

```
$ python3.7 using-generators.py
Traceback (most recent call last):
  File "using-generators.py", line 30, in <module>
    ], f"Expected ['a', 'b', 'c', 'd', 'e'] but got {repr(list(char_range('a', 'e')))}"
AssertionError: Expected ['a', 'b', 'c', 'd', 'e'] but got ['a', 'b', 'c', 'd']
```

We're creating the range well, but we're not yet being inclusive of the stop character.

## Include the Stop Character in the Results

Now that we're yielding characters properly, we need to deviate from how the `range` function works to also include the stop character's code point. To do this, we'll add 1 to the stop character when we are creating our range:

**using-generators.py** (partial)

```
def char_range(start, stop, step=1):
    start_code = ord(start)
    stop_code = ord(stop)

    for value in range(start_code, stop_code + 1, step):
        yield chr(value)
```

Now when we run the file we should see the following error:

```
$ python3.7 using-generators.py
Traceback (most recent call last):
  File "using-generators.py", line 40, in <module>
    ], f"Expected ['e', 'd', 'c', 'b', 'a'] but got {repr(list(char_range('e', 'a')))}"
AssertionError: Expected ['e', 'd', 'c', 'b', 'a'] but got []
```

This error is caused by us not being able to create a range from a higher number to a lower number while using a positive step value.

## Support the Start Code Point Being More Than the Stop Code Point

To support creating a range from a higher Unicode code point to a lower one, we'll need to make our step value negative if we see that the code point for `start` is greater than the code point for `stop`. We can do this by using a conditional and reassigning the `step` variable:

**using-generators.py** (partial)

```
def char_range(start, stop, step=1):
    start_code = ord(start)
    stop_code = ord(stop)

    if start_code > stop_code:
        step *= -1

    for value in range(start_code, stop_code + 1, step):
        yield chr(value)
```

Running the file again we can see that this almost works:

```
$ python3.7 using-generators.py
Traceback (most recent call last):
  File "using-generators.py", line 43, in <module>
    ], f"Expected ['e', 'd', 'c', 'b', 'a'] but got {repr(list(char_range('e', 'a')))}"
AssertionError: Expected ['e', 'd', 'c', 'b', 'a'] but got ['e', 'd', 'c']
```

The issue here is that we're always adding a positive 1 to our `stop` character's code point, even if we're stepping in a negative direction. To get around this, we can store the value that we add to the `stop_code` in a variable and make it negative if we need to negate the `step value`. Here's what this code change looks like:

**using-generators.py** (partial)

```
def char_range(start, stop, step=1):
    stop_modifier = 1
    start_code = ord(start)
    stop_code = ord(stop)

    if start_code > stop_code:
        step *= -1
        stop_modifier *= -1

    for value in range(start_code, stop_code + stop_modifier, step):
        yield chr(value)
```

Now the `stop_modifier` variable will adjust based on our step direction. Running the file one last time, we should see no output at all which indicates that `char_range` meets all of the predefined expectations.

