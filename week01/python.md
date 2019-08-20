##### Week 01 Contents
- Presentation: [Syllabus Review, Course Objectives Discussion](week01/README.md)
- Code: [Python History and Philosophy](week01/python-philosophy.md)
- Code: [Terminal Basics](week01/terminal.md)
- Code: [Python Introduction and Data Manipulation](week01/python.md)
- Components: [Raspberry Pi, Breadboard, Jumper Cables, LED, Resistor, Pushbutton](week01/circuits.md)
- Homework: [Do some research, complete some Python examples, and setup your Raspberry Pi](week01/homework.md)

-----

### Intro to Python

##### Working in Python

The process for creating and running Python code is a bit obtuse. We will need easy access to a directory full of Python files and other assets, a Terminal emulator (the built-in [Terminal](https://support.apple.com/guide/terminal/welcome/mac) application for macOS or Linux, or [Cygwin](https://www.cygwin.com) for Windows) to run our completed code, as well as some sort of text editor for actually writing code (such as the cross-platform [Sublime](https://www.sublimetext.com)).

Setting up your desktop with the following format as a starting point is advisable.

![screen layout](screen.png)

-----

##### Printing to the Console and Running Python Code

The standard [Hello world](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program) program, often used to demonstrate basic competency in a new programming language, is super easy in Python.

```python
print("Hello world")
```

We will need to type that bit of code into a new file in Sublime and save it as the file `hello.py`. To unify our processes, let's all make a new folder called `python_intro` on our Desktop. We can move into this new folder in our Terminal.


> `cd ~/Desktop/python_intro/`

To run our Python code, we simply use the Terminal command `python`.


> `python hello.py`

```
Hello World
```

We have officially written and executed Python code! :clap:

-----

##### Comments

Comments are text that are humand-readable, and ignored by the computer. Take notes by using comments directly in code!

You can write comments in Python by typing the `#` character and following it up with text on a single line.

```python
# this is a note that the computer doesn't see!
print("Hello world") #this prints text to the console
```

Multi-line comments use three quotation characters to begin and end the comment.

```python
"""
This says hello
to everyone
all over the planet
"""

print("Hello " + "World") 
#we can glue text together with the addition sign
```

-----

##### Variables
It is easy to make a container for information, called a `variable` in Python -- and you don't even need to know what kind of information it will store (unlike many other programming languages). The technical term for this behavior is that Python is a *dynamically-typed* language.

```python
name = "Zach"
siblingCount = "1"

print(name + " has " + str(siblingCount) + "sibling")
```

Note that, in order to print `siblingCount`, we must convert it to a String (text) format with `str()`. Variables can hold whatever data, but in Python it is often necessary to force a data type when the information is used.

-----

##### Python Maths
Manipulating numbers is very important for data manipulation and analysis in any programming language.

There are two kinds of numbers in Python:

- Integers are positive or negative counting numbers, and 0: `...-3, -2, -1, 0, 1, 2, 3...`
- Floats are positive or negative real numbers, containing an arbitrary number of decimal points: `3.0, 3.1, 3.14, 3.1415...`

In Python 2...

```python
print(7 + 5) 
#addition - returns 12

print(7 - 5) 
#subtraction - returns 2

print(7 * 5) 
#multiplication - returns 35

print(7 ** 5) 
#exponentiation - returns seven to the fifth power, 16807

print(7 ** .5) 
#exponentiation - returns the square root of seven, 2.6457513110645907

print(7 / 5) 
#integer division - returns 1 (two integers in a division problem will always return an integer!)

print(7.0 / 5) 
#float division - returns 1.4 (the presence of a single float triggers decimal division)

print(7.0 / 5.0) 
#float division - returns 1.4 (two floats also triggers decimal division)

print(7 % 5) 
#modulo - returns 2, the remainder of the integer division problem

print( abs(-7) )
#absolute value - returns 7
```

Note that division behaves a bit strangly, and differently between Python 2 vs Python 3! 

In Python 3...

```python
print(7 / 5) 
#float division - returns 1.4 (two integers in a division problem will always return an integer!)
#python 2 does integer division here

print(7.0 / 5) 
#float division - returns 1.4 (the presence of a single float triggers decimal division)
#same between Python 2 and 3

print(7.0 / 5.0) 
#float division - returns 1.4 (two floats also triggers decimal division)
#same between Python 2 and 3

print(7.0 // 5.0) 
#forced integer division - returns 1
#python 2 returns 1.0, the float version of interger division, which is kind of useless
```

Incrementally changing existing numerical data is also straightforward.

```python
number = 1

number += 12
#number = 13

number += 15
#number = 28

number += 1
#number = 29

number -= 15
#number = 14

number -= 12
#number = 2

print(number)
#returns 2

```

-----

##### Converting Data Types

Very often we need to convert data between different types to do what we need to do. We saw this before with `str()`, which allowed us to convert integers to text strings for printing statements with text and numbers.

```python
print( str(7) )
#returns "7", the text string version of the integer 7

print( str(7.0) )
#returns "7.0", the text string version of the float 7

print( int("7") )
#returns 7

print( float(7) )
#returns 7.0

print( hex(7) )
#returns 0x7, '0x' always indicates a hexadecimal (base 16) number

print( hex(12) )
#returns 0xc, the hexadecimal version of 12
#we can count in hexadecimal...
#0x0,0x1,0x2,0x3,0x4,0x5,0x6,0x7,0x8,0x9,0xa,0xb,0xc,0xd,0xe,0xf,0x10,0x11,0x12,0x13...

print ( oct(7) )
#returns 0o7, '0o' always indicates an octal (base 8) number

print ( oct(12) )
#returns 0o14, 
#we can count to 20 in octals...
#0o0,0o1,0o2,0o3,0o4,0o5,0o6,0o7,0o10,0o11,0o12,0o13,0o14,0o15,0o16,0o17,0o20,0o21,0o22,0o23...

print ( bin(7) )
#returns 0b111, '0b' always indicates a binary (base 2) number

print ( bin(12) )
#returns 0b1100
#we can count to 20 in binary...
#0b0,0b1,0b10,0b11,0b100,0b101,0b110,0b111,0b1000,0b1001,0b1010,0b1011,0b1100,0b1101,0b1110,0b1111,0b10000,0b10001,0b10010,0b10011
```

-----

##### Indentation and Simple Loops
Some languages use curly braces `{}` and parentheses `()` or other formatting characters to determine which code is subordinated to other code. Often, programmers use indentation to show these relationships, as these formatting characters can be easily missed. Python does not use curly braces, and instead just makes use of indentation directly. Note the important colon `:`.

```python
# run a loop 10 times, starting at 0 and ending at 9
for i in range(10) :
    print("Hello " + str(i) + " times")
```

```
Hello 0 times
Hello 1 times
Hello 2 times
Hello 3 times
Hello 4 times
Hello 5 times
Hello 6 times
Hello 7 times
Hello 8 times
Hello 9 times
```

Note the indentation of the print statement, which tells Python that the `print()` code should be included in the loop.

The variable `i` is called the *iterator*. It is, by default increased by one for each loop. Also, by default, our `range()` starts at `0` and ends before the given argument (here 10).

We can control all these behaviors by providing more arguments to `range()`.

```python
# run a loop 10 times, starting at 5 and ending at 11, jumping up by 3
for i in range(5,12,3) :
    print("Hello " + str(i) + " times")
```

```
Hello 5 times
Hello 8 times
Hello 11 times
```

-----

##### Conditions

Conditions work the same way as loops.

A simple conditional might have only a single check.

```python
# weather model
tempThreshold = 72

tempToday = 35

if tempToday < tempThreshold :
    print("It's cold today")
else :
    print("It's hot today")
```

A more advanced conditional might have many branching options. Note that Python uses the strange `elif` rather than spelling out `else if`. 

```python
# better weather model
tempHigh = 85
tempLow = 65

tempToday = 73

if tempToday < tempLow :
    print("It's cold today")
elif tempToday > tempLow and tempToday < tempHigh :
    print("It's pleasant today")
else :
    print("It's hot today")
```

You can use the following comparison operators in conditionals

```
a == b : true if a is exactly equal to b
a < b  : true if a is less than b
a <= b : true if a is less than or equal to b
a >= b : true if a is greater than or equal to b
a > b  : true if a is greater than b
a != b : true if a does not equal b
```

-----

##### Random 

We can add a bit of functionality to the core Python library by *importing* different functionalities. Random numbers cannot be generated by core Python, so we need to bring a bit of additional code into our Python program.

If we want random integers, we can use `random.randint()`.

```python
import random

for i in range(10) :
	#print a random integer between 0 and 5, inclusive
	print(random.randint(0,5))
```

```
0
5
4
4
1
0
1
4
2
1
```

If we want a random number with decimals (called a "floating point number" by developers), we can instead use `random.uniform()`.

```python
import random

for i in range(10) :
	#print a random number, uniformly weighted, between 0 and 1, inclusive
	print(random.uniform(0,1))
```
```
0.8481560825317095
0.4862262467005589
0.7584955629755024
0.1872005446615962
0.3192872946043329
0.5031837342933824
0.5734300895869947
0.7422321630327087
0.8537804130068116
0.0239247940520823
```

-----

##### Lists

Variables can hold many pieces of information as a list, demarcated by square brackets and separated by commas.

```python
myList = ["hello","greetings","good day","hey","allo","good morning"]
```

You can access individual items in a list by their `index` and square brackets. The first item in the list has the index `[0]`, the second item has the index `[1]`, the last item has the index `[-1]`, and the second to last item in the list has the index `[-2]`...

```python
myList = ["hello","greetings","good day","hey","allo","good morning"]

print(myList[2])
print(myList[0])
print(myList[4])
print(myList[-1])
```

```
good day
hello
allo
good morning	
```

You can ask Python to determine the number (or "length") of items in the list.

```python
myList = ["hello","greetings","good day","hey","allo","good morning"]

print( len(myList) )
```

If we run this, the computer will calculate the number of items in the list.

```
6	
```

This is very useful for setting the range in loops, and critical for any data manipulation! 

```python
myList = ["hello","greetings","good day","hey","allo","good morning"]

for i in range( len(myList) ) : 
	print( myList[i] )
```

```
hello
greetings
good day
hey
allo
good morning	
```

We also can query a list for matching data.

```python
myList = ["hello","greetings","good day","hey","allo","good morning"]

alloPlace = myList.index('allo')

print("Allo is index " + str(alloPlace) + " in myList.")
#returns Allo is index 4 in myList.
```

Sometimes, an index is not needed — we just want to know if an item is in a list or not for conditional checking.

```python
myList = ["hello","greetings","good day","hey","allo","good morning"]

if "good morning" in myList :
    print("we have a good morning!")
else :
    print("not a good morning!")

if "good night" in myList :
    print("we have a good night!")
else :
    print("not a good night!")

#returns "we have a good morning!" and "not a good night"
```

-----

##### List Creation and Manipulation
Lists are very flexible, and have several built-in utilities.

Very often, we want to add items or subtract items from lists. 
```python
#lots of data programming work requires the creation of empty lists before later manipulation
myList = []

myList.append('dog')
myList.append('cat')
myList.append('pig')
myList.append('rabbit')

print(myList)
#Python will return ['dog', 'cat', 'pig', 'rabbit']

#remove index 1 from hte l=ist
del(myList[1])

print(myList)
#Python will return ['dog', 'pig', 'rabbit']
```

That above example is pointless, as we could have just created the list we wanted directly. Combining list manipulation and looping shows how this would more commonly be used.

```python
domesticatedList = ['dog', 'cat', 'pig', 'bunny', 'cow', 'sheep', 'chicken', 'goat', 'horse', 'pigeon', 'alpaca', 'koi', 'finch']
wildList = ['wolf', 'wildcat', 'razorback', 'rabbit', 'auroch', 'mouflon', 'junglefowl', 'ibex', 'tarpan', 'rock dove', 'vicuña', 'carp', 'munia']
yearList = [-13000, -8000, -9000, 600, -8000, -7000, -6000, -1000, -3500, -3000, -2400, 1100, 1700]

#empty placeholder list to start
comboList = []

for i in range( len(domesticatedList) ) :
    comboList.append( [ wildList[i] , domesticatedList[i] , yearList[i] ] )
    #yes, lists can contain lists!
    
#comboList now looks like [ ['wolf','dog',-13000],['wildcat','cat',-8000],...]

for i in range( len(comboList) ) :
    #comboList[i] is each sublist, so to get at the data internal to those sublists, we add another index!
    if comboList[i][2] < 0 : 
        yearSuffix = "BCE"
    else :
        yearSuffix = "CE"

    print("The wild " + comboList[i][0] + " was domesticated into the " + comboList[i][1] + " around year " + str(abs(comboList[i][2])) + yearSuffix)

"""
The wild wolf was domesticated into the dog around year 13000BCE
The wild wildcat was domesticated into the cat around year 8000BCE
The wild razorback was domesticated into the pig around year 9000BCE
The wild rabbit was domesticated into the bunny around year 600CE
The wild auroch was domesticated into the cow around year 8000BCE
The wild mouflon was domesticated into the sheep around year 7000BCE
The wild junglefowl was domesticated into the chicken around year 6000BCE
The wild ibex was domesticated into the goat around year 1000BCE
The wild tarpan was domesticated into the horse around year 3500BCE
The wild rock dove was domesticated into the pigeon around year 3000BCE
The wild vicuña was domesticated into the alpaca around year 2400BCE
The wild carp was domesticated into the koi around year 1100CE
The wild munia was domesticated into the finch around year 1700CE
"""
```

We also often need *slices* of a list, a contiguous range of indices but not the whole list. 
```python
myList = ['dog', 'cat', 'pig', 'rabbit', 'ferret', 'cockatiel']

mySubList = myList[2:4]

print(mySubList)
#Python will return ['pig', 'rabbit']
#The sliced list is inclusive of the first index ([2] 'pig' is included), but exclusive of the second index ([4] 'ferret' is not included)
```

##### List Utilities
Lists can be intentionally ordered.

```python
myIntegerList = [3,5,12,27,16,3,22,15,8]

#sort the list ascending
myIntegerList.sort();
print(myIntegerList)
#returns [3, 3, 5, 8, 12, 15, 16, 22, 27]

#flip a list back-to-front
myIntegerList.reverse();
print(myIntegerList)
#returns [27, 22, 16, 15, 12, 8, 5, 3, 3], since we already sorted it!
```

And, because Python is awesome, lists are also compatible with a set of useful mathematical shortcut functions.

```python
myIntegerList = [3,5,6,1,2]

print( sum(myIntegerList) )
#returns 17

print( max(myIntegerList) )
#returns 6

print( min(myIntegerList) )
#returns 1
```

We can also do statistical analysis in Python, though it can be tricky. We'll later use a library for this sort of work.

```python
myFloatList = [3.9,2.2,5.8,6.1,1.0,2.2,5.8,6.8,1.0,1.0,5.8,3.9,6.8,5.8]

#calculate and print the average (mean) of the list
print( sum(myFloatList) / len(myFloatList) )
#returns the average of the list, 4.1499999999999995

#calculate and print the median
#first we sort the list and find the length
myFloatList.sort()
listLength = len(myFloatList)

#if there are 0 items in the list, there is no median
if listLength < 1 :
    print("no median")

#if there are an odd number of items in the list, get the middle item
#the int() is to force Python 2 and 3 compatibility
if listLength % 2 == 1 :
    print( myFloatList[ int(listLength/2) ] )

#if there are an even number of items in the list, average the middle two items
#the 2.0 is to force floating division in Python 2 and 3
else :
    print( ( myFloatList[ int(listLength/2) - 1 ] + myFloatList[ int(listLength/2 ) ]) / 2.0 ) 
    #returns the median, 4.85

#calculate the mode of a list by changing how th max() function considers the counting
print( max(myFloatList, key = myFloatList.count) )
#returns the median, 5.8
```

-----

##### Dictionaries
Often, we want a bit more structure for our data in Python beyond what lists and indices offer. Dictionaries, called "Objects" in many other programming languages, allow us to associate parameters (called "keys") of entities with "values" associated with those parameters. For this reason, dictionaries are sometimes called `key-value stores`, and in other programming languages are often called `objects`. Dictionaries are bounded by curly braces `{}`, keys and values are associated with colons `:`, and multiple keys are separated by commas `,`. 


```python
dog = {"family":"canidae", "genus":"canis", "species":"canis lupus", "subspecies":"canis l. familiaris", "common name":"doggy"}
cat = {"family":"felidae", "genus":"felis", "species":"felis catus", "subspecies":"felis c. familiaris", "common name":"kitty"}
```

We can access values of an object with square brackets; it is very similar to accessing an item in a list by index. Note that you *cannot* access dictionary keys by index number.

```python
dog = {"family":"canidae", "genus":"canis", "species":"canis lupus", "subspecies":"canis l. familiaris", "common name":"doggy"}
cat = {"family":"felidae", "genus":"felis", "species":"felis catus", "subspecies":"felis c. familiaris", "common name":"kitty"}

print( "My " + dog["common name"] + " is part of the zoologic family " + dog["family"] )
```

We can also add keys and values using the same notation

```python
dog = {"family":"canidae", "genus":"canis", "species":"canis lupus", "subspecies":"canis l. familiaris", "common name":"doggy"}
cat = {"family":"felidae", "genus":"felis", "species":"felis catus", "subspecies":"felis c. familiaris", "common name":"kitty"}

dog["order"] = "carnivora"
cat["order"] = "carnivora"

print(dog)
#returns {'family': 'canidae', 'genus': 'canis', 'species': 'canis lupus', 'subspecies': 'canis l. familiaris', 'common name': 'doggy', 'order': 'carnivora'}
```

-----

##### Lists of Dictionaries!

For data modeling purposes, lists of dictionaries should be considered the fundamental and most important data structure. They allow use to keep track of a *population* of *entities*, each of which has its own set of *parameters*.

```python
pets = [ {"family":"canidae", "genus":"canis", "species":"canis lupus", "subspecies":"canis l. familiaris", "common name":"doggy"}, {"family":"felidae", "genus":"felis", "species":"felis catus", "subspecies":"felis c. familiaris", "common name":"kitty"}]

    print( "My " + pets[0]["genus"] 
    #returns "canis"

    print( "My " + pets[1]["subspecies"] 
    #returns "felis c. familiaris"
```

Much more commonly, this sort of operation would occur in a `for` loop.

```python
pets = [ {"family":"canidae", "genus":"canis", "species":"canis lupus", "subspecies":"canis l. familiaris", "common name":"doggy"}, {"family":"felidae", "genus":"felis", "species":"felis catus", "subspecies":"felis c. familiaris", "common name":"kitty"}, {"family":"suidae", "genus":"sus", "species":"sus scrofa", "subspecies":"sus s. domesticus", "common name":"piggy"}, {"family":"leporidae", "genus":"Oryctolagus", "species":"oryctolagus cuniculus", "subspecies":"oryctolagus c. domesticus", "common name":"bunny"} ]

for i in range( len(pets) ): 
	print( "My " + pets[i]["common name"] + " is part of the zoologic family " + pets[i]["family"] )
```

```
My doggy is part of the zoologic family canidae
My kitty is part of the zoologic family felidae
My piggy is part of the zoologic family suidae
My bunny is part of the zoologic family leporidae
```

Note the double bracketing `[i]["common name"]` which tells Python to access a specific *entity* in the list, and then retrieve a specific *value* by key name. 

-----

##### Synthesizing Data from Lists Iteration

Working with data often requires the retention and manipulation of data iteratively built up and evaluated through a complete dataset. For example, we might want to look through a dataset and count the occurences of various items (a *histogram*), determine what the sum of the all entities' specific keyed values, combine multiple datasets, or build an entirely new dataset that is structured differently than the source (a *matrix transposition*).

Very often when doing this sort of work, we need to create intermediate lists and dictionaries.

```python
pets = [ {"order":"carnivora", "family":"canidae", "genus":"canis", "species":"canis lupus", "subspecies":"canis l. familiaris", "common name":"doggy", "count":7}, {"order":"carnivora", "family":"felidae", "genus":"felis", "species":"felis catus", "subspecies":"felis c. familiaris", "common name":"kitty", "count":3}, {"order":"artiodactyla", "family":"suidae", "genus":"sus", "species":"sus scrofa", "subspecies":"sus s. domesticus", "common name":"piggy", "count":13}, {"order":"lagomorpha", "family":"leporidae", "genus":"Oryctolagus", "species":"oryctolagus cuniculus", "subspecies":"oryctolagus c. domesticus", "common name":"bunny", "count":31},{"order":"artiodactyla", "family":"bovidae", "genus":"bos", "species":"bos taurus", "subspecies":"bos t. domesticus", "common name":"moo-cow", "count":14}, {"order":"carnivora", "family":"mustelidae", "genus":"mustela", "species":"mustela putorius", "subspecies":"musetela p. furo", "common name":"doggy", "count":3}]

#let's find which unique orders show up in the dataset 
orderList = []

#loop through pets and create list of 
for i in range( len(pets) ) :
    petOrder = pets[i]["order"]

    #check if we have seen the order before
    if petOrder not in orderList :
        #we haven't seen this order before, so let's add it to the list as a dictionary
        orderList.append(petOrder)

#now orderList = [{'name': 'carnivora'}, {'name': 'carnivora'}, {'name': 'artiodactyla'}, {'name': 'lagomorpha'}]

#let's add some complexity, and count how many times each order shows up in the 'count' key 
orderCount = []

#we need to loop twice! once through the list of orders, and for each order, we loop through the whole list of pets
for i in range( len(orderList) ) :

    #add each order's name to a dictionary, and set the starting count to 0
    orderCount.append({"name":orderList[i], "count":0})

    for j in range( len(pets) ) :
        if orderCount[i]["name"] == pets[j]["order"]:
            orderCount[i]["count"] += pets[j]["count"]

print(orderCount)                
# returns [{'name': 'carnivora', 'count': 13}, {'name': 'artiodactyla', 'count': 27}, {'name': 'lagomorpha', 'count': 31}]
```