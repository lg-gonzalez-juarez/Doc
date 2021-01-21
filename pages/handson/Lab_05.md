---
title: 05. Hands-on Lab 05
tags: [handson]
keywords: sample
#summary: "This is just a sample topic..."
sidebar: python_sidebar
permalink: Lab_05.html
folder: handson
---

# Using Python String Methods

## Introduction

Strings are the primary way that we interact with non-numerical data in programming, and the `str` type in Python provides us with a lot of powerful methods to make working with string data easier. In this hands-on lab, we'll be creating a script that can take a user-provided message and perform various actions on it before printing out those new results.

To feel comfortable completing this lab you'll want to know how to do the following:

Utilize methods on the str class. Watch the "Using String Methods" from the Certified Entry-Level Python Programmer Certification course.
Working with list literals. Watch the "Lists" video from the Certified Entry-Level Python Programmer Certification course.
Using List functions and methods. Watch the "List Functions and Methods" video from the Certified Entry-Level Python Programmer Certification course.

## The Scenario

Our script `variations.py` will allow us to provide a string and then it will present us with some permutations (all lowercase, all 
uppercase, etc.), of that string. The script will also tell us the string's first and last words, when they are sorted alphabetically. We'll need to utilize numerous methods on the `str` class and some of the functions used to sort lists to make all of this this happen. Here's how we want the script to be used:

## Logging In

There are a couple of ways to get in and work with the code. One is to use the credentials provided in the hands-on lab overview page, log in with SSH, and use a text editor in the terminal.

The other is using VS Code in the browser. If you'd like to go this route, then you will need to navigate to the public IP address of the workstation server (provided in the hands-on lab overview page) on port 8080 (example: http://PUBLIC_IP:8080).

If you'd like to use VS Code in the browser then you will need to navigate to the public IP address of the workstation server on port 8080 (example: http://PUBLIC_IP:8080).

Once we are in the server, create variations.py (with either VS Code or a command line text editor) and we can continue.

Create the `variations.py` Script, Make It Executable with python3.7, and Accept User Input

For `variations.py`, we're going to place it in our home directory `(~)` and we want to make sure that we can run it directly. To keep from being completely tied to the path of our python3.7 binary, we want to set up our shebang properly.

Let's create the file and set the shebang:

**~/variations.py**

```
#!/usr/bin/env python3.7

# Python implementation here
```

With the file created, we need to also make sure that it's executable and we can do this using `chmod`:

```
$ chmod u+x ~/variations.py
```

Next, we'll prompt the user for a message and store it off in a variable.

**~/variations.py**

```
#!/usr/bin/env python3.7

message = input("Enter a message: ")
```

Now if we run the script (`./variations.py`) it should prompt us for a message, and then exit without any errors.

## Print the Lowercase, Uppercase, Title Case, and Capitalized Versions of the User's Input

Now that we have the `message` variable, we're going to print a few different things to the screen:

* The lowercase version using `str.lower`
* The uppercase version using `str.upper`
* The title case version using `str.title`
* The capitalized version using `str.capitalize`

Let's use each of these methods combined with the print function:

**~/variations.py**

```
#!/usr/bin/env python3.7

message = input("Enter a message: ")

print("Lowercase:", message.lower())
print("Uppercase:", message.upper())
print("Capitalized:", message.capitalize())
print("Title Case:", message.title())
```

Now we can run the script to make sure that what we've written up to this point is working properly:

```
$ ./variations.py
Enter a message: This Is My Message
Lowercase: this is my message
Uppercase: THIS IS MY MESSAGE
Capitalized: This is my message
Title Case: This Is My Message
```

## Separate the String and Present the Individual Words as a List

For the remaining requirements of our script, we need to work with the individual words. Because of this, we're going to store the words off as a new variable, `words`. We can get the words by using the `str.split` method. After we've separated the message into words, let's also print them out:

**~/variations.py**

```
#!/usr/bin/env python3.7

message = input("Enter a message: ")

print("Lowercase:", message.lower())
print("Uppercase:", message.upper())
print("Capitalized:", message.capitalize())
print("Title Case:", message.title())

words = message.split()
print("Words:", words)
```

Here's the script running so far:

**$ ./variations.py**

```
Enter a message: This is a test Message!
Lowercase: this is a test message!
Uppercase: THIS IS A TEST MESSAGE!
Capitalized: This is a test message!
Title Case: This Is A Test Message!
Words: ['This', 'is', 'a','test','Message!']
```

## Print the Alphabetic First and Last Words from the User's Input

We're going to sort the `words` in the words list alphabetically and save the new list to `sorted_words` by using the `sorted` built-in function. Lastly, we'll print the alphabetic first and last words. Let's do this by indexing to `0` and `-1`.

**~/variations.py**

```
#!/usr/bin/env python3.7

message = input("Enter a message: ")

print("Lowercase:", message.lower())
print("Uppercase:", message.upper())
print("Capitalized:", message.capitalize())
print("Title Case:", message.title())

words = message.split()
print("Words:", words)

sorted_words = sorted(words)
print("Alphabetic First Word:", sorted_words[0])
print("Alphabetic Last Word:", sorted_words[-1])
```

Here's the final script running:

```
$ ./variations.py
Enter a message: This is a test message!
Lowercase: this is a test message!
Uppercase: THIS IS A TEST MESSAGE!
Capitalized: This is a test message!
Title Case: This Is A Test Message!
Words: ['This', 'is', 'a','test','message!']
Alphabetic First Word: This
Alphabetic Last Word: test
```