---
layout: slide-deck
title:  "Learning Python - week 4"
author: Will Rowe
permalink: /learning-python-week4/
theme: ember
transition: slide
---

<section>
<pre><code data-trim data-noescape>

#### Python for biologists and bioinformaticians

***

# Week 4: working with files

***

Will Rowe

---

## Recap...

***

* lists, tuples and dictionaries

* intro to functions

* creating some string algorithms

----

***

### Today's session

***

* solving a problem from last week we didn't get to

* more info on writing functions

* file I/O

* solving some problems (a bit harder than last week)

----

### Locating restriction sites

***

**Given:**
A DNA string of length at most 1 kbp in FASTA format.

**Return:**
The position and length of every reverse palindrome in the string having length between 4 and 12. You may return these pairs in any order.

```
# sample dataset:
>Rosalind_24
TCAATGCATGCGGGTCTATATGCAT

# sample output:
4 6
5 4
6 6
7 4
17 4
18 4
20 6
21 4
```

---

### Solution

***

```python
#!/usr/bin/env python

fasta = ">Rosalind_24\nTCAATGCATGCGGGTCTATATGCATA\n"

# define a reverse complement function
def reverseComplement(sequence):
  reverse_comp = sequence.replace("A", "t").replace("T", "a").replace("C", "g").replace("G", "c").upper()[::-1]
  return reverse_comp

# get just the sequence from the fasta string
sequence = ""
for line in fasta.split("\n"):
  if line.startswith(">") != True:
    sequence += line

# loop through the palindrome sizes. for each loop iteration, loop through the sequence
for length in range(4,13):
  for index in range(len(sequence)-length+1):
    # for each loop, take a slice of the sequence (size = palindrome) and compare it to its reverse complement
    if sequence[index:index+length] == reverseComplement(sequence[index:index+length]):
      print( "{} {}" .format(index+1, length))
```

----

## Functions revisited

***

* functions are blocks of code that can be reused

* we have already been using built-in functions (print(), len() etc.)

* we define them using the **def** keyword, a function name, arguments and a colon:

```python
def myFunc( word ):
  print( word )
```

---

### Function arguments

***

```python
# required arguments
## they are passed in positional order:
def myFunc( arg1, arg2 ):
  ...


# keyword arguments
## allows you to pass arguments in any order
def myFunc( arg1, arg2 ):
  ...

myFunc( arg2="World", arg1="Hello, " )


# default arguments
## default value used if argument not provided
def myFunc( arg1, arg2 = 20 ):
  ...

myFunc( "My age is:" )


# variable-length arguments
## useful if you don't always know how many arguments to define
## the asterisk denotes the variable length argument
def myFunc( arg1, *arg_tuple):
  ...

```

----

## File I/O (input/output)

***

* we have been using the in-built print function to view our program output

* we have been hard-coding our input data into our programs

* we also tried using keyboard input (STDIN - standard input)

```python
myName = input()
```

---

### Opening and closing files

***

* Python has functions and methods for us to manipulate files

* Python has an open function that creates an *object* from a file

* This file object has methods associated with it that allow us to manipulate the file

---

### Opening and closing files

***

```python
# the basic syntax to open a file:
file_object = open(file_name [, access_mode][, buffering])


# for now, we will leave ignore buffering (this will leave it as the system default)
# the access mode tells Python how to open the file (read, write, append, open in binary etc.)
>>> my_file = open("file.txt", "r")


# some basic access modes
r       =       read
rb      =       read in binary format
w       =       write
a       =       append
r+      =       read and write


# we can view the file object's attributes (we'll cover attributes more later)
>>> print( my_file.name )

file.txt

>>> print( my_file.mode )

r


# closing the file flushes the file from memory and will delete all unwritten information
# we close a file using the close method, the basic syntax is:
file_object.close()

```

---

### Reading and writing files

***

```python
# our file object has read and write methods associated with it
# use the write method to write a variable to the open file
file_object.write(my_string)


# use the read method to bytes of data from a file
file_object.read([count])


# we could use the readlines method to read all the lines of a file into a list
lines = file_object.readlines()


# we can just one line using the readline method (we could combine this with a while loop)
line = file_object.readline()


# to avoid loading large files into memory (like readlines does), we can use an iterator
for line in open("my_file.txt"):
  ...

```

---

### Pythonic way of reading files

***

* there are several ways to handle files in Python, we've only covered the basics

* to help us with the Python problems, the following is considered a Pythonic way of reading files line by line

* this is a good way to handle a lot of bioinformatic files and we don't have to worry about closing the file etc. as the loop handles it

```python
with open("genes.fasta", "r") as file_handle:
  for line in file_handle:
    ...

```

----

### Counting point mutations

***

**Given:**
Two DNA strings ss and tt of equal length (not exceeding 1 kbp).

**Return:**
The Hamming distance dH(s,t)dH(s,t).

```
# sample dataset:
GAGCCTACTAACGGGAT
CATCGTAATGACGGCCT

# sample output:
7
```

---

### Solution

***

```python
#!/usr/bin/env python

# the name of our file to read in
my_file = "data.txt"

# a list to hold our two sequences
seqs = []

# a variable to hold our hamming distance value
hamming_distance = 0

# open our file and add each sequence to our list
with open(my_file, "r") as fh:
  for line in fh:
    seqs.append(line.rstrip("\n"))

# create an iterator to move over each position in both our sequences
for i in range(len(seqs[0])):
  # compare bases in each index position, update hamming distance if they differ
  if seqs[0][i] != seqs[1][i]:
    hamming_distance += 1

# print our output
print(hamming_distance)
```

----

</code></pre>
</section>
