---
title: 06. Hands-on Lab Using Python Conditionals
tags: [handson]
keywords: sample
#summary: "This is just a sample topic..."
sidebar: python_sidebar
permalink: Lab_06.html
folder: handson
---

# Using Python Conditionals


## Introduction

The majority of programs that we write will need to be more than a single sequential path of execution. We usually work with data that isn't always the same, and occasionally requires us to do different things based on that data. To achieve this we need to utilize conditionals. In this hands-on lab, we'll work through the conditional logic for the popular interviewing Fizz-Buzz problem by creating a script that will prompt the user for a number and then print out either the number, "Fizz", "Buzz", or "FizzBuzz" depending on whether the number meets one of the Fizz-Buzz requirements.

To feel comfortable completing this lab you'll want to know how to do the following:

Utilize conditionals. Watch the "The if and else Statements" and "Handling Multiple Cases with elif" videos from the Certified Entry-Level Python Programmer Certification course.
Handle user input. Watch the "The input Function" video from the Certified Entry-Level Python Programmer Certification course.

## The Scenario

A typical Fizz-Buzz problem goes like this:

Write a program that prints the numbers from 1 - 100. For each multiple of three print "Fizz" instead of the number. For multiples of five print "Buzz" instead of the number. If a number is a multiple of three and five then print "FizzBuzz".

Solving this problem requires 2 key components: looping and conditionals. We're going to write a simplified program only using the conditional portion that will take any given number that we provide and print the Fizz-Buzz value. Here's how it will work:

```
$ ./fizz-buzz-item.py
Enter an integer value: 15
FizzBuzz
$ ./fizz-buzz-item.py
Enter an integer value: 21
Fizz
$ ./fizz-buzz-item.py
Enter an integer value: 17
17
```

To feel comfortable completing this lab you'll want to know how to do the following:

Utilize conditionals. Watch the "The if and else Statements" and "Handling Multiple Cases with elif" videos from the Certified Entry-Level Python Programmer Certification course.
Handle user input. Watch the "The input Function" video from the Certified Entry-Level Python Programmer Certification course.

## Logging In
There are a couple of ways to get in and work with the code. One is to use the credentials provided in the hands-on lab overview page, log in with SSH, and use a text editor in the terminal.

The other is using VS Code in the browser. If you'd like to go this route, then you will need to navigate to the public IP address of the workstation server (provided in the hands-on lab overview page) on port 8080 (example: http://PUBLIC_IP:8080).

Create the **fizz-buzz-item.py** Script, Make It Executable with python3.7, and Accept User Input
We'll create a fizz-buzz-item.py right in our home directory (~), and we want to make sure that we can run it directly, so that we're not completely tied to the path of our python3.7 binary. We can do this by writing our shebang properly.

Let's create the file and set the shebang:

**~/fizz-buzz-item.py**

```
#!/usr/bin/env python3.7

# Python implementation here
```

With the file created, we need to also make sure that it's executable. We'll do that using chmod:

```
$ chmod u+x ~/fizz-buzz-item.py
```

Next, we'll prompt the user for a value, convert it to an int, and store it off in a variable:

**~/fizz-buzz-item.py**

```
#!/usr/bin/env python3.7

value = int(input("Enter an integer value: "))
Print "FizzBuzz" if the Value Is a Multiple of Three and Five
```

With the value read, we can create our conditional. Since we have a case that should trigger if the value is a multiple of three and five, then we'll want that branch to be before the only multiple of three or only multiple of five branches, because either of those would also be true. Let's create our if statement and print "FizzBuzz" if the condition is met, then add an else to just print the value otherwise:

**~/fizz-buzz-item.py**

```
#!/usr/bin/env python3.7

value = int(input("Enter an integer value: "))

if value % 5 == 0 and value % 3 == 0:
    print("FizzBuzz")
else:
    print(value)
```

Now we can run the script to make sure that what we've written up to this point is working properly:

```
$ ./fizz-buzz-item.py
Enter an integer value: 15
FizzBuzz
$ ./fizz-buzz-item.py
Enter an integer value: 4
4
$
```

Print "Fizz" if the Value Is a Multiple of Three, and "Buzz" if it's a Multiple of Five
To handle the other two cases of the Fizz-Buzz problem, we'll need to utilize elif statements for each of the individual comparisons that we used in our if statement. Let's add those now:

**~/fizz-buzz-item.py**

```
#!/usr/bin/env python3.7

value = int(input("Enter an integer value: "))

if value % 5 == 0 and value % 3 == 0:
    print("FizzBuzz")
elif value % 3 == 0:
    print("Fizz")
elif value % 5 == 0:
    print("Buzz")
else:
    print(value)
```

Now we can run the script to make sure that what we've written up to this point is working properly:

```
$ ./fizz-buzz-item.py
Enter an integer value: 15
FizzBuzz
$ ./fizz-buzz-item.py
Enter an integer value: 4
4
$ ./fizz-buzz-item.py
Enter an integer value: 9
Fizz
$ ./fizz-buzz-item.py
Enter an integer value: 10
Buzz
$
```
