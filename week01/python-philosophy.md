##### Week 01 Contents
- Presentation: [Syllabus Review, Course Objectives Discussion](readme.md)
- Code: [Python History and Philosophy](python-philosophy.md)
- Code: [Terminal Basics](terminal.md)
- Code: [Python Introduction and Data Manipulation](python.md)
- Components: [Raspberry Pi, Breadboard, Jumper Cables, LED, Resistor, Pushbutton](circuits.md)
- Homework: [Do some research, complete some Python examples, and setup your Raspberry Pi](homework.md)

-----

### Python History and Philosophy

Python is a very powerful and easy to learn programming language, which can be used for applications as [diverse](https://www.python.org/success-stories/) as website coding, robotics, software development for desktop and mobile applications, firmware development for embedded devices, generative design, scientific computing, video and special effects for movies, and system architecture. In combination with various tools for data [analysis](http://www.numpy.org), [information extraction and manipulation](https://pandas.pydata.org), [statistical modeling](https://www.scipy.org), [data journalism presentation](https://jupyter.org), [machine learning](https://www.tensorflow.org), and [visualization ](https://matplotlib.org) libraries — is the language sitting at the heart of the contemporary explosion in data-driven modeling.

![python logo](https://www.python.org/static/img/python-logo@2x.png)

##### Short History of Python

The late 1980's were an awkward time in computer programming. The ease of development of the early world wide web via the early specifications for HTML and PHP were attracting newcomers into programming. Other common programming languages available at the time like C++ and BASIC — designed for more traditional and broader programming applications — effectively demanded tremendous exposure to computer science principles and best practices before they could be used by amateur programmers. Tutorials and learning resources for these traditional languages were scarce, but the quick diffusion of web programming knowledge through informal websites and chat rooms and the ease of website creation exposed that an appetite and ability for programming existed outside of inwards-looking computer science communities.  

![bdfl](https://upload.wikimedia.org/wikipedia/commons/d/d0/Guido-portrait-2014-curvves.jpg)

[Guido van Rossum](https://en.wikipedia.org/wiki/Guido_van_Rossum), a Dutch computer scientist, saw this discrepancy in 1991 and began developing a language to help transition programmers away from the declarative languages of the web and towards the use of more powerful languages — and that effort was named [Python](https://en.wikipedia.org/wiki/Python_(programming_language)) after Van Rossum's favorite British comedy group [Monty Python](https://en.wikipedia.org/wiki/Monty_Python). Not :snake:. Many references to comedy work from the group exist in Python tutorials, and the Python community holds humorous punning as one of its core values. Van Rossum is currently, and as an in-joke always will be, the [benevolent dictator for life (BDFL)](https://en.wikipedia.org/wiki/Benevolent_dictator_for_life) of Python — though it is a fully [open source project](https://en.wikipedia.org/wiki/Open_source) with thousands of contributors across the planet.

![monty python](http://www.comedycentral.co.uk/sites/default/files/styles/image-w-520-scale/public/cc_uk/galleries/large/2015/10/14/902-monty-python-and-the-holy-grail-quotes.gif?itok=Bwb6BNHE)

Python is designed to be easy to learn and human-readable. Towards this end, whitespace (tabs, space, and returns) is semantic in Python unlike in most other programming languages. Additional eccentricities include [optional data typing](https://en.wikipedia.org/wiki/Type_system#DYNAMIC), easy transitions between [functional](https://en.wikipedia.org/wiki/Functional_programming) and [object-oriented]https://en.wikipedia.org/wiki/Object-oriented_programming() programming paradigms, and a comprehensive but easily extensible [standard library](https://en.wikipedia.org/wiki/Standard_library).

-----

##### Python's Philosophy

The [Zen of Python](https://en.wikipedia.org/wiki/Zen_of_Python), a note posted to the official Python message board in 1995 by developer [Tim Peters](https://en.wikipedia.org/wiki/Tim_Peters_(software_engineer)), was adopted in 1995 as the official guiding logic of the Python project.

- Beautiful is better than ugly.
- Explicit is better than implicit.
- Simple is better than complex.
- Complex is better than complicated.
- Flat is better than nested.
- Sparse is better than dense.
- Readability counts.
- Special cases aren't special enough to break the rules.
- Although practicality beats purity.
- Errors should never pass silently.
- Unless explicitly silenced.
- In the face of ambiguity, refuse the temptation to guess.
- There should be one — and preferably only one — obvious way to do it.
- Although that way may not be obvious at first unless you're Dutch.
- Now is better than never.
- Although never is often better than *right* now.
- If the implementation is hard to explain, it's a bad idea.
- If the implementation is easy to explain, it may be a good idea.
- Namespaces are one honking great idea — let's do more of those!

Many of these guidelines should be familiar to designers! When writing in Python, we should do our best to keep these ideas in mind.

-----

### Digital Development + Python

##### Our Approach

We will be learning Python [*immersively*](https://en.wikipedia.org/wiki/Language_immersion), as we might learn a foreign language directly while traveling through a foreign country without having had a formal education in the local language. We will pick up only as much grammar and vocabulary as is required to complete the task at hand, and we will do our best to quickly and repeatedly re-use and flex those acquired linguistic elements — even in situations where there might be a better or more *native* way.

This approach will yield quick confidence and fast progress without the mind-numbing boredom of learning programming languages in the abstract, but will also leave large gaps in our knowledge. We will also find that the code we are learning seems to only be usable for what we are doing at any given moment, with little obvious [*extensibility*](https://en.wikipedia.org/wiki/Extensibility) towards other programming ends.

Homework and project development will be used to fill some of these holes, but ultimately only regular practice will yield any real fluency. Free beginner and advanced tutorials for Python are common for just about any computer programming goal, and everyone is encouraged to try out their burgeoning Python skills on outside-of-class projects to shore up their expertise. Recommendations will be provided week-to-week.

###### Versioning

One unique challenge of learning Python is an ongoing versioning bifurcation. Python 3, the current version of the language, strove to simplify and standardize the previous versions of Python. It is, however, imcompatible in small and large ways from Python 2, which has a rich and diverse ecosystem of tutorials and libraries. As a result, Python 2 and Python 3 are both still in use by developers — and some developers even use both version in different aspects of the same project. Many efforts towards pushing the world towards Python 3 exclusively have been attempted, with little success. We will mostly be writing code that is valid in both Python 2 and 3, though many of the libraries we are using will be designed for Python 3.
