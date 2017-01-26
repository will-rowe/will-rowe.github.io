---
layout: slide-deck
title:  "Learning Python - week 1"
author: Will Rowe
categories: main
group: bioinformatics
permalink: /learning-python-week1/
theme: ember
transition: slide
---

<section data-markdown data-separator="^\n----\n" data-separator-vertical="^\n---\n">
#### Python for biologists and bioinformaticians

***

# Week 1: an introduction to Python

***

Will Rowe

---

## Aims of these talks

***

 * To be able to read Python code

 * To be able to write basic Python

 * To know when and how to use it in our day-to-day research

 * Work together on developing / debugging projects

---

## Assumptions

***

 * You have Python (3) installed

 * The Python interpreter is set in your PATH variable

 * You want to learn Python

----

***

### Today's session

***

 * Some programming terminology

 * Background to Python

 * Python syntax

 * Variables

 * Getting started with Python

 * Hello world . . .

----

## Basic terminology

***

| Term | Meaning |
| :----------- | :----------- |
|||
| programming | giving a computer a set of instructions to perform |
|||
| code | the instructions in a computer program |
|||
| interpreter | a computer program that executes code from a programming language |
|||
| syntax | the set of rules for a given programming language |
|||
| shell | a user interface |
|||
| prompt | an indicator (symbol) that the computer is waiting for input |
|||
| object-oriented programming | programming model based on 'objects' that define both the data type (in attributes) and the operations (functions) that can be applied to the data structure |
|||
| expression | programming instruction consisting of values and operators |

----

## What is Python?

***

 * An interpreted coding language
  - no (explicit) compilation step
  - the Python interpreter reads code and executes it

 * A high-level language

 * Object-oriented programming

 * Two [versions](https://www.python.org/downloads/) available (2 & 3)
  - Python 2.x is legacy, Python 3.x is the present and future of the language

---

## What is Python?

***

 * Created in the '80s by Guido van Rossum (Netherlands)

 * Name comes from Monty Python

 * See the [Zen of Python](https://www.python.org/dev/peps/pep-0020/) for basic principles / philosophy
  - Readability counts
  - Explicit is better than implicit

---

## Why Python?

***

 * Easy to learn & use
  - easy syntax
  - lots of tools to help you
  - open source / free
  - high-level language - (e.g. don't have to allocate memory)

 * Rapid prototyping

 * Lots of functionality (standard libraries, PyPi etc.)

 * Good starting place for other languages

---

## Who uses Python?

***

 * Web:
  - Google
  - Yahoo maps

 * Film and games:
  - Industrial Light and Magic (StarWars!)
  - Battlefield2

 * Science:
  - NASA
  - National Weather Service
  - Bioinformaticians everywhere

----

## Python Syntax

***

We'll cover a few of the basic syntax rules here...

---

***

### Indentation

***

 * line indentation denotes code blocks (for flow control, defining functions etc.)

 * indentation is enforced (program will fail if indentation is wrong)

 * blank lines are ignored

```python
# good indentation:
for i in [1,2,3]:
        print(i)

# bad indentation:
for i in [1,2,3]:
print(i)
```

---

***

### Identifiers

***

 * Identifier = a **name** used to identify a variable, function, class, module or other object

 * They start with **A-Z**, **a-z** or **_** and followed (optionally) by letters, digits and underscores

 * There are naming conventions, such as:
  - class names start with uppercase letter
  - starting an identifier with an underscore means the identifier is private

---

***

### Identifiers cont.

***

 * Python has a set of keywords that cannot be used as identifiers:

| | | | |
| ---- | ---- |  ---- |  ---- |  
| and | exec | not | except |
| assert | finally | or | lambda |
| break | for | pass | yield |
| class | from | print | else |
| continue | global | raise | is |
| def | if | return | with |
| del | import | try | while |
| elif | in | | |

---

***

### Identifiers cont.

***

 * Example:

```python
# good identifiers:
a = 1
bee = 'insect'
item1 = 'banana'

# bad identifiers:
1variable = 'bug'
and = 'bug2'
```

A syntax error looks like this:

```python
  File "<stdin>", line 1
    1variable = 'bug'
            ^
SyntaxError: invalid syntax
```

---

***

### Comments, quotation and docstrings

***

 * Comments begin with a **#** (outside a string literal)
  - everything after # is ignored by Python **until** the end of the line

 * Quotes, either single ('), double (") and triple (''' / """), denote string literals
  - the same type must be used to begin/end a string literal
  - triple quotes span a string across multiple lines

 * A docstring is a string literal that occurs as the first statement in a module, function, class, or method definition.

> A literal is a succinct and easily visible way to write a value

----

## Variables

***

 * A variable is a reserved location in the computer’s memory where you can store a single value
  - can store results from evaluated expressions to use later

 * You don't need to declare variables
  - python assigns them (allocates memory)

 * Variables are references to objects
  - everything is an object!
  - datatype is a property of an object, not the variable

---

***

### Assigning values

***

 * We use an **=** to assign a value to a variable identifier:

```python
name = "Grace"       # a string object
age = 1              # an integer object
kilometers = 123.5   # a floating point number object
```

 * We can assign multiple variables at once:

```python
name, age, kilometers = 'Grace', 1, 123.5
```

---

***

### Data types

***

 * Python has standard **data types** that define what we can do with them and how they are stored:

|||
| :----------- | :----------- |
|||
| Number | numeric values (including: int, long, float and complex) |
|||
| String | a set of characters |
|||
| List | a set of items (can be different types of items)|
|||
| Tuple | like a list but **immutable** |
|||
| Dictionary | contain key:value pairs of values |

---

***

### Examples of data types

***

```python
# number
an_int = 1
a_float = 666.6

# string (must be enclosed in quotes)
a_str = 'Hello everyone!'

# list (must be enclosed in square brackets)
a_list = [ 'angus', 666 , 28.5, 'SG', 1400.99 ]

# tuple (must be enclosed in parentheses)
a_tuple = ( 'angus', 666 , 28.5, 'SG', 1400.99 )

# dictionary
a_dictionary = {} # this is an empty dictionary
a_dictionary['key_1'] = 'this is the first value entered'

one_line_dict = {'name': 'Angus', 'band': 'AC/DC', 'guitar': 'SG'}
```

we'll cover how to manipulate these data types later . . .

----

## Recap . . .

***

| Term | Meaning |
| :----------- | :----------- |
|||
| variable | reserved memory location to store values |
|||
| immutable | unable to be changed |
|||
| identifier | a name used to identify a variable, function, class, module or other object |
|||
| literal | a value written exactly as it's meant to be interpreted |
|||
| data types | number, string, list, tuple, dictionary |

----

## Getting started with Python

***

### Shell mode vs. program mode

***

* Shell mode lets you type Python expressions into the **Python shell / prompt**
  - this is interactive
  - good for working out quick problems

* Program mode lets you give the Python interpreter an entire program
  - doesn't automatically display results from the program

---

***

### Shell mode

***

To start Python in shell mode, type:

```python
python
```

You should see something like this:

```python
Python 3.5.0 (v3.5.0:374f501f4567, Sep 12 2015, 11:00:19)
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

The three chevron symbol (```>>>```) is the Python prompt. To quit the shell, type:

```python
quit()
```

---

***

### Program mode

***

We run a python program in this mode by:

```python
python my_program.py
```

 * We type *python* to tell the computer that we want the Python interpreter to run the file.

 * The file *my_program.py* contains the Python code that we want to run. We will only see output if we have told Python (as part of the program code).

 * The extension *.py* is a convention that tells our computer that the file contains Python code.

----

## Hello World

***

We'll finish off by running a typical 'first program'

---

Our file (*my_program.py*) contains this:

```python
#!/usr/bin/env python           # this 'hashbang' line says what interpreter to use

print('Hello world!')           # this is a print statement (prints to STDOUT)

print('What is your name?')

myName = input()                # this is a way to get user input and assign it to a variable

print('It is good to meet you, ' + myName)

print('What is your age?')

myAge = input()

expression_output = str(int(myAge) + 1) # this is an example of storing output from an expression

print('You will be {} in a year.' .format(expression_output)) # this is an example of printing a variable
```

>side note - we have given our program file permission to allow execution

---

To run our program, type:

```python
python my_program.py
```

You should see something like this:

```python
Hello world!
What is your name?

```

----

## Next time . . .

***

There was a lot of terminology etc. this week - but once we have got this down, reading/writing Python is relatively straight forward!

Next time we will do some more exciting stuff now we know the basics

 * Basic expressions

 * Manipulating data types

 * Case study of a real-world, simple, problem

----

</section>


























<section>
    <pre><code data-trim data-noescape>
    </code></pre>
</section>
