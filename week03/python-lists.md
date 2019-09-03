##### Week 03 Contents
- Presentation: [Inclusive Design Small Group Chats, Electrical Signalling](readme.md)
- Components: [RGB LED Color Controller](circuits.md)
- Code: [Python Lists and Dictionaries](python-lists.md)
- Code: [Python GPIO Control](python-gpio.md)
- Application: [Train Arrival Warning Alarm](application.md)
- Homework: [Readings, Other APIs, Programming Practice](homework.md)
    
-----

### Lists and Dictionaries

##### Lists

A single Python variable can hold many pieces of information as a *list*, demarcated by square brackets and separated by commas. The same structure is called an *array* in many other computing contexts.

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

This is very useful for setting the range in loops, and critical for any real data manipulation where you are, for example, accessing an API and you have no way of knowing how many data-points the server might return.

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

-----

##### List Lookups
We can query a list for matching data.

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

That above example is contrived, as we could have just created the list we wanted directly. Combining list manipulation and looping shows how this would more commonly be used.

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

#calculate the mode of a list by changing how the max() function works 
print( max(myFloatList, key = myFloatList.count) )
#returns the mode, 5.8
```

-----

##### Dictionaries
Often, we want a bit more structure for our data in Python beyond what lists and indices offer. Dictionaries allow us to associate parameters (called "keys") of entities with "values" associated with those parameters. For this reason, dictionaries are sometimes called `key-value stores`, and in other programming languages are often called `objects`. Dictionaries are bounded by curly braces `{}`, keys and values are associated with colons `:`, and multiple keys are separated by commas `,`. 


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

Very often when doing this sort of work, we need to create intermediate lists and dictionaries. This is difficult, and confusing, and will take time to be able to replicate on your own. Work line by line, `print()` alot, and try to imagine how the data is mutating with each operation.

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