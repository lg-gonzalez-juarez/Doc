---
title: 27. Hands-on Lab Reading and Writing Files with Python
tags: [handson]
keywords: sample
#summary: "This is just a sample topic..."
sidebar: python_sidebar
permalink: Lab_27.html
folder: handson
---

## Introduction

Files are used for many things in programming, including storing and reading data as well as writing to the screen. In this hands-on lab, we'll add a way to read and store information about custom classes, using a file as a flat database for our employee information. To feel comfortable completing this lab, you'll want to know how to read and write to files (watch the "Interacting with Files" video from the Certified Associate in Python Programming Certification course), use class methods (watch the "Custom Constructors, Class Methods, and Decorators" video from the Certified Associate in Python Programming Certification course), and create and use class instances (watch the "Creating and Using Python Classes" video from the Certified Associate in Python Programming Certification course).

## Solution

To work through the lab, you can either log in via a terminal on your local machine and use a text editor in the terminal, or you can use VS Code in the browser. This lab guide will go through the steps using VS Code in the browser.

In order to use VS Code in the browser, navigate to the public IP address of the workstation server (provided on the lab page) on port 8080 (e.g., http://<PUBLIC_IP>:8080).

## Add identifier Attribute to Employee Instances and __init__ Method

1. In the menu at the top, click **File** > **Open**.

2. Select **employee.py**.

3. In the `__init__ function`, within the parentheses, add a comma after `phone_number=None`  and add:

```
identifier=None
```

4. Under `self.phone_number = phone_number`, add the following:

```
self.identifier = identifier
```

## Add Employee.get_all Class Method to Return a List of Employee Objects

1. Edit the beginning of the `employee.py` file to match the following:

```
class Employee:
    default_db_file = "employee_file.txt"

    @classmethod
    def get_all(cls, file_name=None):
        results = []

        if not file_name:
            file_name = cls.default_db_file

        with open(file_name, "r") as f:
            lines = [
                line.strip("\n").split(",") + [index + 1]
                for index, line in enumerate(f.readlines())
            ]

        for line in lines:
            results.append(cls(*line))

        return results

    # remainder of class was unchanged and omitted
```

2. In the menu at the top, click **Terminal** > **New Terminal**.

3. Run `test_employee.py`:

```
python3.7 test_employee.py
```

We'll get an error that we're missing the `get_at_line` attribute.

## Add Employee.get_at_line Class Method to Return a Single Employee

1. Add the following beneath the `get_all` class method:

```
    @classmethod
    def get_at_line(cls, line_number, file_name=None):
        if not file_name:
            file_name = cls.default_db_file

        with open(file_name, 'r') as f:
            line = f.readlines()[line_number - 1]
            attrs = line.strip("\n").split(',') + [line_number]
            return cls(*attrs)

    # remainder of class was unchanged and omitted
```

2. Run `test_employee.py`:

```
python3.7 test_employee.py
```

We'll get an error that we're missing the save attribute.

## Add save Instance Method to Employee Class to Write New Instances to the File

1. Add the following to the bottom of the file (beneath the `def email_signature` block):

```
    def save(self, file_name=None):
        if not file_name:
            file_name = self.default_db_file

        with open(file_name, "r+") as f:
            lines = f.readlines()
            if self.identifier:
                lines[self.identifier - 1] = self._database_line()
            else:
                lines.append(self._database_line())
            f.seek(0)
            f.writelines(lines)

    def _database_line(self):
        return (
            ",".join(
                [self.name, self.email_address, self.title, self.phone_number or ""]
            )
            + "\n"
        )
```

2. For clarity's sake, this is what the `employee.py` file should look like at this point:

```
class Employee:
    default_db_file = "employee_file.txt"

    @classmethod
    def get_all(cls, file_name=None):
        results = []

        if not file_name:
            file_name = cls.default_db_file

        with open(file_name, "r") as f:
            lines = [
                line.strip("\n").split(",") + [index + 1]
                for index, line in enumerate(f.readlines())
            ]

        for line in lines:
            results.append(cls(*line))

        return results

    @classmethod
    def get_at_line(cls, line_number, file_name=None):
        if not file_name:
            file_name = cls.default_db_file

        with open(file_name, "r") as f:
            line = [
                line.strip("\n").split(",") + [index + 1]
                for index, line in enumerate(f.readlines())
            ][line_number - 1]
            return cls(*line)

    def __init__(self, name, email_address, title, phone_number=None, identifier=None):
        self.name = name
        self.email_address = email_address
        self.title = title
        self.phone_number = phone_number
        self.identifier = identifier

    def email_signature(self, include_phone=False):
        signature = f"{self.name} - {self.title}\n{self.email_address}"
        if include_phone and self.phone_number:
            signature += f" ({self.phone_number})"
        return signature

    def save(self, file_name=None):
        if not file_name:
            file_name = self.default_db_file

        with open(file_name, "r+") as f:
            lines = f.readlines()
            if self.identifier:
                lines[self.identifier - 1] = self._database_line()
            else:
                lines.append(self._database_line())
            f.seek(0)
            f.writelines(lines)

    def _database_line(self):
        return (
            ",".join(
                [self.name, self.email_address, self.title, self.phone_number or ""]
            )
            + "\n"
        )
```

3. Test the implementation by running `test_employee.py`.

```
python3.7 test_employee.py
```

If the implementation is correct, we won't see any errors. If things aren't working correctly, we will see error messages that can hopefully help us.
