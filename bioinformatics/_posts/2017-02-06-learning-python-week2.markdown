---
layout: slide-deck
title:  "Learning Python - week 2"
author: Will Rowe
permalink: /learning-python-week2/
theme: ember
transition: slide
---

<section data-markdown data-separator="^\n----\n" data-separator-vertical="^\n---\n">
#### Python for biologists and bioinformaticians

***

# Week 2: variables, operators and control structures

***

Will Rowe

---

## Recap...

***

 * Some programming terminology & background to Python

 * Python syntax

 * Variables

 * Getting started with Python & Hello world

----

***

### Today's session

***

 * Manipulating variables

 * Python operators

 * Control structures

 * Using the above to solve some problems

----

## Manipulating variables

***

 * We covered what variables were last week and how to assign identifiers to variables

 * We also know that variables can be different data types (number, string, list etc.)

 * Let's show some examples of how we can manipulate different types of variables

---

### String manipulation

***

Use square brackets (`[]`) to access characters in a string

```python
>>> my_string = "hello world"
>>> first_letter = my_string[0]
>>> print( first_letter )

h
```

**note that Python uses 0-based indexing!**

---

### String manipulation

***

Also use `[]` to take a slice of a string

```python
>>> my_string = "hello world"
>>> print( my_string[0:3] )

hel

>>> print( my_string[:3] )

hel

>>> print( my_string[6:] )

world
```

Slice notation:

[ first element to include : first element to exclude : step ]

---

### String manipulation

***

We can use Python's built-in functions and string methods

```python
# return length of string
len(my_string)

# return the number of occurences for a query
my_string.count('e')

# return the index of the query
my_string.find('e')

# split the string on a given delimiter
my_string.split(' ')

# strip characters from string
my_string.strip()     #removes from both ends
my_string.lstrip()    #removes leading characters (Left-strip)
my_string.rstrip()    #removes trailing characters (Right-strip)

# return True if string starts/ends with query
my_string.startswith('H')
my_string.endswith('H')

# replace
my_string.replace('Hello', 'Goodbye')

# change case
my_string.upper()
my_string.lower()

# join (add new character between each string character)
':'.join(my_string)

# reverse string
''join(reversed(my_string))

# string testing
my_string.isalnum()         #check if all char are alphanumeric
my_string.isalpha()         #check if all char in the string are alphabetic
my_string.isdigit()         #test if string contains digits
my_string.istitle()         #test if string contains title words
my_string.isupper()         #test if string contains upper case
my_string.islower()         #test if string contains lower case
my_string.isspace()         #test if string contains spaces
my_string.endswith('d')     #test if string endswith a d
my_string.startswith('H')   #test if string startswith H
```

---

### Data type conversion

***

We can use built-in functions to convert between types. Here are some examples:

```python
# convert x to an integer (base specifies the base if x is a string)
int(x [,base])

# convert to floating-point
float(x)

# convert to string
str(x)

# convert to a list
list(x)
```

---

We'll cover more on manipulating variables next week . . .

----

## Python operators

***

 * Symbols that carry out arithmetic / logical computation

 * Python has lots of operator types:
  - arithmetic
  - comparison / relational
  - logical
  - assignment
  - bitwise, membership, identity (we'll do these another time)

---

### Arithmetic operators

***

| Operator | Description | Example |
| :----------- | :----------- | :----------- |
||||
| + | adds values on either side of the operator | a + b = 30 |
||||
| - | subtraction | a - b = 10 |
||||
| * | multiplication | a * b = 200 |
||||
| / | division | a / b = 2 |
||||
| % | modulus - divides and then returns remainder | a % b = 0 |
||||
| ** | exponent | a**b = 10 to the power of 20 |
||||

---

### Comparison / relational

***

| Operator | Description
| :----------- | :----------- |
|||
| == | equals |
|||
| != | does not equal |
|||
| > | greater than |
|||
| < | less than |
|||
| >= | greater than or equal to |
|||
| <= | less than or equal to |
|||

---

### Logical

***

| Operator | Description
| :----------- | :----------- |
|||
| and | if both operands true, condition is true |
|||
| or | if one of two operands is true, condition is true |
|||
| not | reverses logical state of its operand |
|||

---

### Assignment operators

***

| Operator | Description | Example | Equivalent to |
| :----------- | :----------- | :----------- | :----------- |
||||
| = | assigns values from the right to the left of the operand | c = a + b | |
||||
| += | adds the right to the left, then assigns to the left | c += a | c = c + a |
||||
| -= | subtracts the right to the left, then assigns to the left | c -= a | c = c - a |
||||
| *= | multiplies the right to the left, then assigns to the left | c *= a | c = c * a |
||||
| /= | divides the right to the left, then assigns to the left | c /= a | c = c / a |
||||
| %= | takes modulus using right and left, then assigns to the left | c %= a | c = c % a |
||||
| **= | performs exponential calculation using right and left, then assigns to the left | c **= a | c = c ** a |
||||

----

## Control structures

***

In order for our programs to flow, Python has two important control structures:

 * iteration
 * selection

---

***

### Iteration

***

 * Python has the `while` and `for` statements for iteration

 * The `while` statement iterates over a body of code as long as a condition is true

 * The `for` statement iterates over the members of a collection
  - any expression in the `while` and `for` statements are evaluated first

---

```python
# while loops:
>>> counter = 1
>>> while counter <= 5:
...     print( "Hello, world" )
...     counter = counter + 1


Hello, world
Hello, world
Hello, world
Hello, world
Hello, world


# for loops:
>>> for letter in 'Python':
       print( 'Current Letter :', letter )

Current Letter : P
Current Letter : y
Current Letter : t
Current Letter : h
Current Letter : o
Current Letter : n

>>> fruits = ['banana', 'apple',  'mango']
>>> for fruit in fruits:
       print( 'Current fruit :', fruit )

Current fruit : banana
Current fruit : apple
Current fruit : mango


# iterating for loop by index:
>>> fruits = ['banana', 'apple',  'mango']
>>> for index in range(len(fruits)):
       print( 'Current fruit :', fruits[index])

Current fruit : banana
Current fruit : apple
Current fruit : mango
```

---

***

### Selection

***

 * Python has the `if` and `else` statements for iteration

 * Allow different actions to be performed based on the result of the if/else statement

 * Selection control statements can be nested, and can also utilise statements such as `elif`

---

```python
# if statement:
if n<0:
   print("Sorry, value is negative")
else:
   print(math.sqrt(n))


# utilising elif instead of nested loops
if score >= 90:
   print('A')
elif score >=80:
   print('B')
elif score >= 70:
   print('C')
elif score >= 60:
   print('D')
else:
   print('F')
```

---

We'll cover more on control structures another time . . .

----

## Rosalind problem

***

"Rosalind is a platform for learning bioinformatics and programming through problem solving"

---

### Counting DNA nucleotides

***

**Given:**
A DNA string ss of length at most 1000 nt.

**Return:**
Four integers (separated by spaces) counting the respective number of times that the symbols 'A', 'C', 'G', and 'T' occur in ss.

```
Sample Dataset
AGCTTTTCATTCTGACTGCAACGGGCAATATGTCTCTGTGTGGATTAAAAAAAGAGTGTCTGATAGCAGC

Sample Output
20 12 17 21
```

---

### Solution

***

```python
#!/usr/bin/env python

string = "TGGAGAGTTAGCAAGACTAGTACTACCCGAACTTTTGAACGGGATGTAACAATACTCGTAATGAACTAGGCCTATCGTATAAAGTGACGCCGGAATTTCCTAAGGCTGGGCAATCTTTGTTCATCCAAGCTACAGAAGCGGATAGACCTTCACTCCAAGCGAACCATCCTTATGGGCACTCATGGGGGTTATTGAGTCACCGACAGATTGAAAACTCCTTACATCCCACTACGCGGTAGACCGTGCCTTGATTTCCTGTTAAGATGCATGATCAGGAGTGGGCACCAATTTACCCAAACTCCCGGGAAGCCGTATTAAACGTACGCTCTAGGCATCTTTGTGTGGCCAGCACGGAGTTGAGAGAAATGAGGGCAGGGTGCTAGGCTTCAGCGGACCATCTGTTTTTCGCAGGGAAATCATACAGCCTGCTAGGTCGAGTTGGCGCCCTTGACGCTCAACGACAGTGGATCGGCAACCATTCATAGTACTCTTGGTCGGAAGGGACGTCTATGTAAATACGAGGTAGCAGTTTGCCAGCCGGTACTCTATGTCGGCCGGCGTGTACAGCCTTCCTTGACATGAAAAAAGAGATTAAAGTCGCGGTATCGCCTGCTGCCAGACTCGTTGACAGTGGAACTTCTTGTACTGGAACGCCGCCGTCGATAGTCGTCTCGCAGAGTCGTGCACTGAATGACCGTTATTAGTAGAGGCGTGTAAGGAAGTCTGGGGGGGACCGGGAAATTTTGGGTGATGCGTACAAGCATAGGTCCTACGTTAGGTGGCTTATTCTTCACTGGGCACATATAATAGGTTCAACATGTCGATTCGTGGACCTCGCACATTTTGCGTAGATCACCT"

print( sring.count("A"), string.count("G"), string.count("C"), string.count("T") )

```

---

### Solution 2

***

```python
#!/usr/bin/env python

string = "TGGAGAGTTAGCAAGACTAGTACTACCCGAACTTTTGAACGGGATGTAACAATACTCGTAATGAACTAGGCCTATCGTATAAAGTGACGCCGGAATTTCCTAAGGCTGGGCAATCTTTGTTCATCCAAGCTACAGAAGCGGATAGACCTTCACTCCAAGCGAACCATCCTTATGGGCACTCATGGGGGTTATTGAGTCACCGACAGATTGAAAACTCCTTACATCCCACTACGCGGTAGACCGTGCCTTGATTTCCTGTTAAGATGCATGATCAGGAGTGGGCACCAATTTACCCAAACTCCCGGGAAGCCGTATTAAACGTACGCTCTAGGCATCTTTGTGTGGCCAGCACGGAGTTGAGAGAAATGAGGGCAGGGTGCTAGGCTTCAGCGGACCATCTGTTTTTCGCAGGGAAATCATACAGCCTGCTAGGTCGAGTTGGCGCCCTTGACGCTCAACGACAGTGGATCGGCAACCATTCATAGTACTCTTGGTCGGAAGGGACGTCTATGTAAATACGAGGTAGCAGTTTGCCAGCCGGTACTCTATGTCGGCCGGCGTGTACAGCCTTCCTTGACATGAAAAAAGAGATTAAAGTCGCGGTATCGCCTGCTGCCAGACTCGTTGACAGTGGAACTTCTTGTACTGGAACGCCGCCGTCGATAGTCGTCTCGCAGAGTCGTGCACTGAATGACCGTTATTAGTAGAGGCGTGTAAGGAAGTCTGGGGGGGACCGGGAAATTTTGGGTGATGCGTACAAGCATAGGTCCTACGTTAGGTGGCTTATTCTTCACTGGGCACATATAATAGGTTCAACATGTCGATTCGTGGACCTCGCACATTTTGCGTAGATCACCT"

# create a dictionary to count our bases
freq = {'A': 0, 'C': 0, 'G': 0, 'T': 0}

# use a for loop to iterate through the characters in our string
for i in string:
    # for each character, check it is in our dictionary
    # and then increase the value using an arithmetic expression
    freq[i] += 1

# print out the results in the format requested
print( freq['A'], freq['C'], freq['G'], freq['T'])
```

---

### More solutions (other languages)

***

```go
# in R
string <-"ACTGCGTGCA"
table(strsplit(string, NULL)[[1]])


# Perl
!/usr/bin/perl
use strict;

while (<STDIN>)
{
my $string=$_;
my $a=($_=~tr/A//);
my $b=($_=~tr/C//);
my $c=($_=~tr/G//);
my $d=($_=~tr/T//);
print  "$a,$b,$c,$d\n";
}


# Go
package main
import "fmt"

func main() {
    var my_string = []byte("ACTGCGTGCA")
    var my_map [256]int
    for _, base := range my_string {
        my_map[base]++
    }
    fmt.Println(my_map['A'], my_map['C'], my_map['G'], my_map['T'])
}
```

----

## Complementing a strand of DNA

***

**Given:**
A DNA string ss of length at most 1000 bp.

**Return:**
The reverse complement scsc of ss.

***

**Sample dataset:**
AAAACCCGGT

**Sample output:**
ACCGGGTTTT

---

### Solution

***

```python
#!/usr/bin/env python

string = "TGGAGAGTTAGCAAGACTAGTACTACCCGAACTTTTGAACGGGATGTAACAATACTCGTAATGAACTAGGCCTATCGTATAAAGTGACGCCGGAATTTCCTAAGGCTGGGCAATCTTTGTTCATCCAAGCTACAGAAGCGGATAGACCTTCACTCCAAGCGAACCATCCTTATGGGCACTCATGGGGGTTATTGAGTCACCGACAGATTGAAAACTCCTTACATCCCACTACGCGGTAGACCGTGCCTTGATTTCCTGTTAAGATGCATGATCAGGAGTGGGCACCAATTTACCCAAACTCCCGGGAAGCCGTATTAAACGTACGCTCTAGGCATCTTTGTGTGGCCAGCACGGAGTTGAGAGAAATGAGGGCAGGGTGCTAGGCTTCAGCGGACCATCTGTTTTTCGCAGGGAAATCATACAGCCTGCTAGGTCGAGTTGGCGCCCTTGACGCTCAACGACAGTGGATCGGCAACCATTCATAGTACTCTTGGTCGGAAGGGACGTCTATGTAAATACGAGGTAGCAGTTTGCCAGCCGGTACTCTATGTCGGCCGGCGTGTACAGCCTTCCTTGACATGAAAAAAGAGATTAAAGTCGCGGTATCGCCTGCTGCCAGACTCGTTGACAGTGGAACTTCTTGTACTGGAACGCCGCCGTCGATAGTCGTCTCGCAGAGTCGTGCACTGAATGACCGTTATTAGTAGAGGCGTGTAAGGAAGTCTGGGGGGGACCGGGAAATTTTGGGTGATGCGTACAAGCATAGGTCCTACGTTAGGTGGCTTATTCTTCACTGGGCACATATAATAGGTTCAACATGTCGATTCGTGGACCTCGCACATTTTGCGTAGATCACCT"

# string is manipulated left to right - first replacing, then changing case, then reversing using a slice
print( string.replace('A', 't').replace('T', 'a').replace('C', 'g').replace('G', 'c').upper()[::-1] )




# instead of a slice, we could have reversed it this way:
print( ''.join(reversed(string.replace('A', 't').replace('T', 'a').replace('C', 'g').replace('G', 'c').upper())) )
```

----

## There's more than one solution?

***

**Zen of Python:**

There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.

---

 * Is it readable?

 * Does it perform well?
  - `'foo'[::-1]` performs better than ```''.join(reversed('foo'))```
  - reverse slice is generally faster than calling a method on a called function
  - should we consider things like memory access vs file I/O?
  - does the performance matter?

 * "Pythonic means code that doesn't just get the syntax right but that follows the conventions of the Python community and uses the language in the way it is intended to be used"

----

</section>






<section>
    <pre><code data-trim data-noescape>
    </code></pre>
</section>
