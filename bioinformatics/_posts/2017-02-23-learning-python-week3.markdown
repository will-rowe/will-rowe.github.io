---
layout: slide-deck
title:  "Learning Python - week 3"
author: Will Rowe
permalink: /learning-python-week3/
theme: ember
transition: slide
---

<section data-markdown data-separator="^\n----\n" data-separator-vertical="^\n---\n">
#### Python for biologists and bioinformaticians

***

# Week 3: lists, tuples and dictionaries

***

Will Rowe

---

## Recap...

***

 * Manipulating variables

 * Python operators

 * Control structures

 * Using the above to solve some problems

----

***

### Today's session

***

 * More info on some basic data types - lists, tuples and dictionaries

 * Quick intro to functions

 * Solving some problems (a bit harder than last week)

----

## Lists, tuples and dictionaries

***

 * We've already covered the other two basic data types in detail - strings and numbers

 * Python's data types can be grouped by structure (numeric, sequences, sets and mappings)

 * Sequences are a structure that have every element of the data assigned to an index

 * Strings are a **sequence** of unicode characters

---

### Lists

***

Lists are sequences (like strings)

You can **index**, **slice**, **add/subtract from**, **multiply** and **check** them

```python
# use index
>>> my_list = ["sg", "les paul", "explorer"]
>>> print( my_list[2] )

explorer

# take slice
>>> my_tuple = ("1", "2", "3", "banana")
>>> print( my_tuple[2:4] )

('3', 'banana')
```

---

### Lists

***

Update / delete one or more elements of a list using slice notation

```python
# update element
>>> my_list = ["sg", "les paul", "explorer"]
>>> my_list[2] = "flying v"
>>> print( my_list )

['sg', 'les paul', 'flying v']

# delete element
>>> del my_list[0:2]
>>> print( my_list )

['flying v']
```

**remember that tuples are immutable so can't be altered**

---

### Lists

***

Perform list operations like you would do to strings

```python
# concatenate
>>> print( ["sg", "les paul", "explorer"] + ["flying v"] )

['sg', 'les paul', 'explorer', 'flying v']

# multiply
>>> print( ["sg"] * 3 )
['sg', 'sg', 'sg']

# iterate
>>> for x in ["sg", "les paul", "explorer"]: print(x)

sg
les paul
explorer

# check membership
>>> print( "sg" in ["sg", "les paul", "explorer"] )

True
```

---

### Lists

***

We can use Python's built-in list functions and methods

```python
# return length of list
len(my_list)

# return max / min value in list
max(my_list)
min(my_list)

# convert tuple to list
list(my_tuple)

# compare elements in lists
cmp(my_list, another_list)


# list methods
my_list.append(item)      #append item to a list
my_list.remove(item)      #remove an item from a list
my_list.count(item)       #count occurences of item in a list
my_list.extend(my_tuple)  #append a list/tuple to a list
my_list.sort()            #sort list by elements
my_list.reverse()         #reverse the order of a list
my_list.pop()             #remove the last item from list
my_list.index(item)       #returns the lowest index for item in list
my_list.inset(index, item)#insert item at given index
```

---

### Tuples

***

In brief, tuples are like lists but immutable

You can't change a tuple but you can create a new one from a previous tuple/s

You can largely perform operations and functions as for lists but the list methods don't apply

---

### Dictionaries

***

Python dictionaries are map structures (aka. hashmaps or associative arrays)

Put simply, each element (key) in a dictionary list is associated with a value

Keys must be unique but values don't have to be

(standard python) dictionaries are **unordered**

---

### Dictionaries

***

Dictionaries are notated by curly braces

Keys and values are separated by a colon, dictionary elements are separated by a comma

```python
# empty dictionary
>>> my_dict = {}

# dictionary with elements
>>> my_dict2 = {"Name": "Angus", "Guitars": "SG"}

# dictionary with list of values
>>> my_dict3 = {"Name": "Jimmy", "Guitars": ["Les Paul", "SG"]}
>>> print( my_dict3 )

{'Name': 'Jimmy', 'Guitars': ['Les Paul', 'SG']}

# add an item to a dictionary
>>> my_dict3["Amp"] = "Orange AD30"
>>> print( my_dict3 )

{'Amp': 'Orange AD30', 'Name': 'Jimmy', 'Guitars': ['Les Paul', 'SG']}

# access dictionary value by key
>>> print( my_dict3["Name"] )

Jimmy

# for loop to print all values in dict
>>> for x in my_dict3: print( my_dict3[x] )

Orange AD30
Jimmy
['Les Paul', 'SG']
```

---

### Dictionaries

***

There are a set of built-in functions and methods that we can use with dictionaries

We'll look at these another time, along with more complex examples of dictionaries

----

## Functions

***

* functions are blocks of code that can be reused

* they also allow your code to be modular - i.e. separates functionality

* we have already been using built-in functions (print(), len() etc.)

---

### Functions

***

 A basic intro to writing a function:

```python
# We define functions using the def keyword, then the function name, parentheses and a colon
>>> def my_function():

# We can give parameters to the function
>>> def my_function( param1, param2 ):

# The function code block (after the colon) must be indented
>>> def my_function( param1, param2 ):
...    print( param1 )
...    print( param2 )

# Functions can return a value(s)
>>> def my_function( my_number ):
...    my_new_number = my_number * 2
...    return my_new_number

# To call a function and save the return value
>>> my_result = my_function( 1 )
>>> print( my_result )

2
```

----

## Rosalind problem

***

"Rosalind is a platform for learning bioinformatics and programming through problem solving"

---

### Finding a motif in DNA

***

**Given:**
Two DNA strings ss and tt (length <= 1 kbp).

**Return:**
All locations of tt as a substring of ss.

```
# sample dataset:
GATATATGCATATACTT
ATAT

# sample output:
2 4 10
```

---

### Solution

***

```python
#!/usr/bin/env python

ss = "GATATATGCATATACTT"
tt = "ATAT"


# using what we learned about lists today:

# make a list to save our positions
start_pos = []

# turn ss into a list and create an index variable
ss_list = list(ss)
index = 0

# move along the elements in the list
for i in ss_list:

  # use slice notation to grab four bases from the current position in ss
  # and then compare it to tt (we need to convert the ss slice back to a string)
  if ( "".join(ss_list[index:index+len(tt)]) == tt ):

    # if they match, save the current index position in ss + 1
    # (the numbering in the output uses 1-based indexing)
    start_pos.append(str(index+1))

  # increment the index by one
  index += 1

# print the answer
print( " ".join(start_pos))


###
# However, a better way is to use the xrange function
# allowing us to move along ss by index position
###
start_pos = []
for i in xrange(0, len(ss)-1):
  if (ss[i:i+len(tt)] == tt):
    start_pos.append(str(i+1))

print( " ".join(start_pos))

```

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

fasta = ">Rosalind_24\nTCAATGCATGCGGGTCTATATGCAT\n"


# define a reverse complement function
def reverseComplement(sequence):
  reverse_comp = sequence.replace("A", "t").replace("T", "a").replace("C", "g").replace("G", "c").upper()[::-1]
  return reverse_comp


# get just the sequence from the fasta string
sequence = ""
for line in fasta.split("\n"):
  if line.startswith(">") != True:
    sequence += line

# starting at position 0 of the sequence, test for palindromes of 4 bases
# increment through to 12 palindromic bases
# once exhausting 4-12, increment one position through the sequence
for i in xrange(0, len(sequence)-1):
  # decrease palindrome length and move on when one is found
  for palindrome_len in reversed(xrange(4, 13)):
    if palindrome_len > len(sequence[i:]):
      continue
    if sequence[i:palindrome_len] == reverseComplement(sequence[i:palindrome_len]):
      print( "{} {}" .format(i+1, len(sequence[i:palindrome_len])) )
      print("\n\n")
      break



```

----

</section>




<section>
    <pre><code data-trim data-noescape>
    </code></pre>
</section>
