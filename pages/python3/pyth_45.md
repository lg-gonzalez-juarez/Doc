---
title: 45. Input and Output Basics
tags: [python]
keywords: sample
#summary: "This is just a sample topic..."
sidebar: python_sidebar
permalink: pyth_45.html
folder: python3
---

## 45.1. Interacting with Files

There are some core actions that we need to understand how to do in any programming language, in order to be very productive. One of these actions is interacting with files. In this lesson, we'll learn how to read from and write to files, and we'll take a look at how bytes can be represented in code.

### Documentation

- [The open function](https://docs.python.org/3/library/functions.html#open)
- [The file object](https://docs.python.org/3/glossary.html#term-file-object)
- [The io module](https://docs.python.org/3/library/io.html#io-overview)
- [Bytes Objects](https://docs.python.org/3.7/library/stdtypes.html#bytes-objects)

### Files as Objects

One of the beautiful aspects of working in an object-oriented programming language is that we can represent concepts as objects with functionality. Files are a great use case for this. Python gives us the [file object](https://docs.python.org/3/glossary.html#term-file-object) (or concept, really). These objects provide us a few things:

* A `read` method to access the underlying data in the file
* A `write` method to place data into the underlying file

To test this out, we're going to create a simple text file with some names it in, and then read and modify it to see what we can learn.

### Opening a File

The first step to interacting with a file is to "open" it, and in Python, we'll use the [open function](https://docs.python.org/3/library/functions.html#open). This function takes two main arguments:

* `file` - The path to the file on disk (or where you'd like to create it)
* `mode` - How you would like to interact with the file

The `file` argument is pretty simple, but the `mode` argument has a variety of options that all work a little differently:

* `'r'` - Opens the file for reading, which is the default mode
* `'w'` - Opens the file for writing, while removing the existing content (truncating the file)
* `'x'` - Opens the file to create it, failing if the file already exists
* `'a'` - Opens the file for writing without truncating, appending any new writes to the end of the file
* `'b'` - Opens the file in binary mode, in which the file expects to write and return bytes objects
* `'t'` - Opens the file in text mode, the default mode, where the object expects to write and return strings
* `'+'` - Opens the file for reading and writing

These modes can be used in combination, so `w+b` is a valid mode saying that we want to read and write with bytes, and with the existing file being truncated (from the w).

Let's create a new script called `using_files.py` (within a new directory called `file_io`), and we'll start interacting with a file containing some names. The file doesn't exist yet, but if it did, we'd like to truncate it and prepare to write to it.

`~/file_io/using_files.py`

```
my_file = open('xmen.txt', 'w+')
```

Now we have a new file object that we can write to.

### Writing to the File

Before we can read from our file, we need it to have some content. There are a few primary methods that we'll interact with depending on whether or not we want to work with lines or individual characters. The write method only writes the characters that we specify, where the writelines method takes a list of strings that should all be on their own line. Let's add some names to our file, each on its own line, using both methods:

`~/file_io/using_files.py`

```
my_file = open('xmen.txt', 'w+')
my_file.write('Beast\n')
my_file.write('Phoenix\n')
my_file.writelines([
    'Cyclops',
    'Bishop',
    'Nightcrawler',
])
```

Let's save the file, run it, and then check the contents of `xmen.txt`:

```
$ python3.7 using_files.py
$ cat xmen.txt
Beast
Phoenix
CyclopsBishopNightcrawler
$
```

This isn't quite what we expected. You would probably think that `writelines` would add a new line with each entry, but we still need to add the `'\n'` to the end of each item. The `writelines` method is more of a shorthand for multiple calls to `write`, however you can use `newline='\n'` when we opened the file.

Another thing that we didn't do is close the file. When we're finished working with a file, we should call the `close` method. It's not necessary when running this script because the filehandle will be closed when the program terminates. But when we're interacting with files, from within a server, for instance, the program won't terminate for a long time.

### Reading from a File

Now that we have some content in the file, let's close it within the script and then re-open it for reading.

```
~/file_io/using_files.py

my_file = open('xmen.txt', 'w+')
my_file.write('Beast\n')
my_file.write('Phoenix\n')
my_file.writelines([
    'Cyclops\n',
    'Bishop\n',
    'Nightcrawler\n',
])
my_file.close()

my_file = open('xmen.txt', 'r')
print(my_file.read())
my_file.close()
```

Now we can run the script again to see what happens:

```
$ python3.7 using_files.py
Beast
Phoenix
Cyclops
Bishop
Nightcrawler
$
```

Since we're reading the file in 'text' mode, we'll receive a single string from the `read` method that contains the newline characters, and when printed, it will print the newlines accordingly. If we didn't want this parsing to occur, we could work with the file in `bytes` mode.

If we were to call the `read` method again, we would receive an empty string in response. This is because the file holds onto a cursor for the location within the file, and when we read, it returns everything after that cursor's position, and moves the cursor to the end. To reread the existing content, we'll need to use `seek` to move within in the file. Additionally, as an alternative to `read` we can use `readlines` to return a list of lines in the file (this can allow you to work with the data a little easier). Here's an example of both of these things in action.

`~/file_io/using_files.py`

```
my_file.write('Beast\n')
my_file.write('Phoenix\n')
my_file.writelines([
    'Cyclops\n',
    'Bishop\n',
    'Nightcrawler\n',
])

my_file.seek(0)
my_file.write('Morph')
my_file.seek(0)
for line in my_file.readlines():
    print(line)

my_file.close()
This would output:

$ python3.7 using_files.py
Morph

Phoenix

Cyclops

Bishop

Nightcrawler

$
```

### The with Statement

Remembering to close files that we opened can be tedious, and to get around this, Python gives us the `with` statement. A `with` statement takes an object that has a `close` method and will call that method after the block has run.

Let's rewrite our existing code to utilize the `with` statement:

`~/file_io/using_files.py`

```
with open('xmen.txt', 'w+') as my_file:
    my_file.write('Beast\n')
    my_file.write('Phoenix\n')
    my_file.writelines([
        'Cyclops\n',
        'Bishop\n',
        'Nightcrawler\n',
    ])

my_file = open('xmen.txt', 'r')
with my_file:
    print(my_file.read())
```

When we open the file to write, we're using the shorthand `as` expression to open the file within the `with` statement, and assigning it to the variable `my_file` within the block. This is a really handy tool if we don't need to use the file in any other way. An alternative would be to create the `my_file` variable manually and then pass the variable into the `with` statement like we did when we were reading from the file.

## 45.2. Working with Bytes

Now that we know how to work with files at a base level, we need to know how to work with `bytes` and `bytearray` objects since they're so closely related to file IO.

### Documentation 

- [The open function](https://docs.python.org/3/library/functions.html#open)
- [The file object](https://docs.python.org/3/glossary.html#term-file-object)
- [The io module](https://docs.python.org/3/library/io.html#io-overview)
- [Binary Sequence Types](https://docs.python.org/3.7/library/stdtypes.html#binary-sequence-types-bytes-bytearray-memoryview)
- [Bytes Objects](https://docs.python.org/3.7/library/stdtypes.html#bytes-objects)
- [Bytearray Objects](https://docs.python.org/3.7/library/stdtypes.html#bytearray-objects)

### What is a bytes Object?

A [bytes object](https://docs.python.org/3.7/library/stdtypes.html#bytes-objects) is an immutable sequence (just like a string) that consists of single bytes of data. Where a string is a sequence of characters, a `bytes` object is a sequence of bytes. There's also a type called [bytearray](https://docs.python.org/3.7/library/stdtypes.html#bytearray-objects) that is a mutable counterpart of the `bytes`. A "byte" is an integer from 0 to 256, but we will see them represented using ASCII characters and hexadecimal numbers. Let's create our first `bytes` object in the REPL:

```
>>> my_bytes = b"This is a byte"
>>> my_bytes
b'This is a byte'
```

The `b` prefix allows us to create a `bytes` object literal, but we can also use the `bytes()` constructor method:

```
>>> bytes(b"This is a byte")
b'This is a byte'
>>> bytes(10)
b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
>>> bytes(range(10))
b'\x00\x01\x02\x03\x04\x05\x06\x07\x08\t'
```

What is `\x00?` A 2 character hexadecimal number encompasses all possible values for an individual byte (0-256). The bytes object will show its representation using 2 digit hexadecimal values unless the value translates to an ASCII character like `\x09` being `\t`.

A big difference between strings and bytes is what is returned when you index the item versus slicing. For a string, you always get a string returned, even for a single index. For bytes, indexing will return an integer; slicing will return a bytes object.

```
>>> my_bytes
b'This is a byte'
>>> my_bytes[0]
84
>>> my_bytes[0:2]
b'Th'
```

### Bytearrays

There isn't a literal syntax for creating a `bytearray` object; we need to use the `bytearray` constructor, which works just like the `bytes` constructor except that it can also take a bytes object as the argument to create a mutable version.

```
>>> bytearray()
bytearray(b'')
>>> bytearray(10)
bytearray(b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00')
>>> bytearray(range(10))
bytearray(b'\x00\x01\x02\x03\x04\x05\x06\x07\x08\t')
>>> bytearray(b'Bytes')
bytearray(b'Bytes')
```

Because bytearrays are mutable, we can use the same assignment operations that we would normally with a list, except that we need to remember that when working with an index, we need to pass in an integer value and when replacing a slice we need to pass in a bytes object.

```
>>> b_array = bytearray(10)
>>> b_array[0] = b'T'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'bytes' object cannot be interpreted as an integer
>>> b_array[0:1] = b'T'
>>> b_array
bytearray(b'T\x00\x00\x00\x00\x00\x00\x00\x00\x00')
>>> b_array[1] = 0x10
>>> b_array
bytearray(b'T\x10\x00\x00\x00\x00\x00\x00\x00\x00')
```

### Using bytes Mode for Files

To use the `bytes` mode when reading a file, we'll need to specify whether we're going to read, write, or append and then also add the `b` mode. Let's open our `xmen.txt` file in bytes mode:

```
>>> with open('xmen.txt', 'rb') as my_file:
...     my_file.read()
...
b'Beast\nPhoenix\nCyclops\nBishop\nNightcrawler\n'
>>> with open('xmen.txt', 'rb') as my_file:
...     my_file.readlines()
...
[b'Beast\n', b'Phoenix\n', b'Cyclops\n', b'Bishop\n', b'Nightcrawler\n']
```

The main difference between text mode and bytes mode is that we need to use bytes objects when writing, and we'll receive bytes objects when reading.

Reading into Bytearrays
Because bytearrays are mutable, we can create them specifying the size and then `read` that exact amount of information from a file to place into the bytearray.

```
>>> my_file = open('xmen.txt', 'rb')
>>> b_array = bytearray(10)
>>> my_file.readinto(b_array)
10
>>> b_array
bytearray(b'Beast\nPhoe')
```

An alternative way to create a bytearray with a specific length and content would be to call `read` with the length argument that we hadn't used yet and then passing that value into the `bytearray` constructor:

```
>>> new_b_array = bytearray(my_file.read(10))
>>> new_b_array
bytearray(b'nix\nCyclop')
```

This isn't something that is used all that often, but there's a chance that you might see questions about reading into bytearray objects on the PCAP exam.

{% include links.html %}
