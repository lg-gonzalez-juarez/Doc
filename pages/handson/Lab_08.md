---
title: 08. Hands-on Lab Defining and Using Python Functions
tags: [handson]
keywords: sample
#summary: "This is just a sample topic..."
sidebar: python_sidebar
permalink: Lab_08.html
folder: handson
---

# Defining and Using Python Functions

## Introduction

Functions allow us to package together lines of code to make a process of some kind repeatable action. If there is ever a workflow that we might want to do more than once, then gathering that workflow into a function might be a good idea. In this hands-on lab, we'll be working through exercises to demonstrate creating functions that will receive arguments and return results that meet our expectations.

To feel comfortable completing this lab you'll want to know how to do the following:

Defining functions. Watch "Defining and Using Functions" video from the Certified Entry-Level Python Programmer Certification course.
## The Scenario

Functions are one of the fundamental building blocks for writing well-factored, understandable code. Another benefit of functions is that we're able to package up code so that we don't need to think about the actual logic within the function. We can focus on the results, depending on our inputs. Viewing our functions as black boxes where we provide inputs and receive outputs means that we can also write automated tests to ensure that our functions (which might not even be written yet) behave the way that we expect. This is known as Test-Driven Development. We can use the `assert` statement to write some basic "tests" that utilize a function and ensure that the output is what we expect. We're going to work through the **testing-functions.py** file, writing and modifying functions to get the assertions throughout the file so that it doesn't throw errors.

When we run the **testing-functions.py** file we should see errors like this:

```
$ python3.7 testing-functions.py
Traceback (most recent call last):
  File "testing-functions.py", line 3, in <module>
    assert split_name("Kevin Bacon") == {
NameError: name 'split_name' is not defined
```

This process will show us the line where the issue was encountered, and show us the differences between our expected and actual values.

## Logging In

There are a couple of ways to get in and work with the code. One is to use the credentials provided in the hands-on lab overview page, log in with SSH, and use a text editor in the terminal.

The other is using VS Code in the browser. If you'd like to go this route, then you will need to navigate to the public IP address of the workstation server (provided in the hands-on lab overview page) on port 8080 (example: http://PUBLIC_IP:8080).

## Create the split_names Function to Separate Names

The first few tasks require us to create the `split_names` function to take a string and split it into the first and last name, returning a dictionary with the `first_name` and `last_name keys`.

We'll modify the function to get each of the assert statements that utilize this, so let's write the implementation to get the initial assertion to succeed:

**testing-functions.py**

```
# 1) Write a `split_name` function that takes a string and returns a dictionary with first_name and last_name

def split_name(name):
    names = name.split()
    first_name = names[0]
    last_name = names[-1]
    return {
        'first_name': first_name,
        'last_name': last_name,
    }

assert split_name("Kevin Bacon") == {
    "first_name": "Kevin",
    "last_name": "Bacon",
}, f"Expected {{'first_name': 'Kevin', 'last_name': 'Bacon'}} but received {split_name('Kevin Bacon')}"
```

Now when we run the file again we'll see the next error:

```
$ python3.7 testing-functions.py
Traceback (most recent call last):
  File "testing-functions.py", line 19, in <module>
    }, f"Expected {{'first_name': 'Victor', 'last_name': 'Von Doom'}} but received {split_name('Victor Von Doom')}"
AssertionError: Expected {'first_name': 'Victor', 'last_name': 'Von Doom'} but received {'first_name': 'Victor', 'last_name': 'Doom'}
```

This test shows that our initial implementation doesn't handle having multiple spaces in the initial `name`. Thankfully, to get around this, we can assume that we only need to split on the first space in the string. This is something that we can do with the `str.split` function by passing an additional argument with the number of splits to perform.

**testing-functions.py** (partial)

```
# 1) Write a `split_name` function that takes a string and returns a dictionary with first_name and last_name

def split_name(name):
    first_name, last_name = name.split(maxsplit=1)
    return {
        'first_name': first_name,
        'last_name': last_name,
    }

assert split_name("Kevin Bacon") == {
    "first_name": "Kevin",
    "last_name": "Bacon",
}, f"Expected {{'first_name': 'Kevin', 'last_name': 'Bacon'}} but received {split_name('Kevin Bacon')}"

# 2) Ensure that `split_name` can handle multi-word last names

assert split_name("Victor Von Doom") == {
    "first_name": "Victor",
    "last_name": "Von Doom",
}, f"Expected {{'first_name': 'Victor', 'last_name': 'Von Doom'}} but received {split_name('Victor Von Doom')}"
```

Running the tests again, we shouldn't see any more errors related to `split_name`.

## Create the is_palindrome Function to Determine if a String or Number Is a Palindrome

A palindrome is a word that reads the same from right to left as it does from left to right (or reversed). An example is 'radar'. The word 'radar' reversed is still 'radar'. We need to create a function called `is_palindrome` that takes a string and checks to see if it matches itself in reverse. The simplest way that we can do this is by using slicing to reverse the string:

**testing-functions.py** (partial)

# 3) Write an `is_palindrome` function to check if a string is a palindrome (reads the same from left-to-right and right-to-left)

```
def is_palindrome(item):
    return item == item[::-1]

assert is_palindrome("radar") == True, f"Expected True but got {is_palindrome('radar')}"
assert is_palindrome("stop") == False, f"Expected False but got {is_palindrome('stop')}"

# 4) Make `is_palindrome` work with numbers

assert is_palindrome(101) == True, f"Expected True but got {is_palindrome(101)}"
assert is_palindrome(10) == False, f"Expected False but got {is_palindrome(10)}"
```

This will get #3 to succeed, but when we hit the assertions under #4 we'll see the following errors:

```
$ python3.7 testing-functions.py
Traceback (most recent call last):
  File "testing-functions.py", line 31, in <module>
    assert is_palindrome(101) == True, f"Expected True but got {is_palindrome(101)}"
  File "testing-functions.py", line 23, in is_palindrome
    return item == item[::-1]
TypeError: 'int' object is not subscriptable
```

We can fix this by type casting our `item` parameter to be a string before we make our comparison. This will also make it work with floats like 1.1:

**testing-functions.py** (partial)

```
# 3) Write an `is_palindrome` function to check if a string is a palindrome (reads the same from left-to-right and right-to-left)

def is_palindrome(item):
    item = str(item)
    return item == item[::-1]

assert is_palindrome("radar") == True, f"Expected True but got {is_palindrome('radar')}"
assert is_palindrome("stop") == False, f"Expected False but got {is_palindrome('stop')}"

# 4) Make `is_palindrome` work with numbers

assert is_palindrome(101) == True, f"Expected True but got {is_palindrome(101)}"
assert is_palindrome(10) == False, f"Expected False but got {is_palindrome(10)}"
```

## Create the build_list Function to Take an Item and a Count and Return a List with item Repeated count Times

The `build_list` function is going take an `item` and the number of times to add it to a list that it will return. Based on the second assertion that we can see in the file, the `count` should also have a default value of 1.

Let's write a function to build a `range`, then loop through it and append the item to a list that we'll return at the end of the function.

**testing-functions.py** (partial)

```
# 5) Write a `build_list` function that takes an item and a number to include in a list

def build_list(item, count=1):
    items = []
    for _ in range(count):
        items.append(item)
    return items

assert build_list("Apple", 3) == [
    "Apple",
    "Apple",
    "Apple",
], f"Expected ['Apple', 'Apple', 'Apple'] but received {repr(build_list('Apple', 3))}"
assert build_list("Orange") == [
    "Orange"
], f"Expected ['Orange'] but received {repr(build_list('Orange'))}"
```