##### Week 02 Contents
- Presentation: [Installation, Inclusive Design Conversation](readme.md)
- Code: [Moving around with Terminal](terminal.md)
- Code: [Python Basics](python-basics.md)
- Homework: [Readings, API Access, Programming Practice](homework.md)

-----

### Terminal Basics

Before we can get to Python, we need to learn how to use our computers in a slightly different way. Let's begin to interact with machines through programmatic text in a [command line interface (CLI)](https://en.wikipedia.org/wiki/Command-line_interface). 

On Macs or Linux machines, open `Terminal` from the `Applications/Utilities Folder`. 

On Windows, install [Cygwin](https://www.cygwin.com)

The Terminal is a way of controlling your computer without any graphical user elements. You type in commands, and the machine responds with text back to you.

macOS and Cygwin use a Terminal dialect closely [Bash](https://en.wikipedia.org/wiki/Bash_(Unix_shell)) which is derived from a shared Unix ancestry. The Bash language that we will be writing is [very concise](http://ss64.com/bash/).

#### Using These Tutorials

A quick note: code that you are designed to type and execute on this page looks like this...

> `cd ~/Desktop`

The computer's response will always be formatted like this...

```
Desktop
Documents
```

-----

##### The Prompt

After Terminal launches, it will present you with a prompt in this format. 

```
ComputerName:~ user$ 
```

For example, if your computer's name was `robot` and your current user name was `human`, you would see `robot:~ human$`.

The `~` tells us where we are in our file system. More on that later.

-----

##### Getting Some Feedback

After the `$` you can type in commands and then hit `enter`, try the following

> `echo hello`

Your computer will return

```
hello
```

The `echo` command is followed by a space and an `argument`. Spaces are semantic in Bash, and separate arguments from their inputs. Any argument that `echo` takes, it spits back out. Wrapping longer arguments in quotation marks ensures that the echo command speaks back all of its input together.

> `echo "hello goodbye"`
 
```
hello goodbye
```
 
-----

##### Locating Ourselves 

Enter another Bash command 

> `pwd` 

which will return something like

```
/Users/zach
```

but with your username instead of `zach`

`pwd` stands for 'print working directory,' and returns where your Bash shell is currently located in your computer's filesystem. Your Bash shell opens into your user home directory, which is represented in Bash by the `~` (tilda) character. This is why your prompt all along has included the tilda character `~` — it is showing you where you are currently located in your computer's storage.

```
ComputerName:~ user$ 
```

See the tilda (`~`)?

-----

##### Browsing

We can see what is in the current working directory with `ls`.

> `ls`

```
Applications
Desktop
Documents
Downloads
Library
Movies
Music
Pictures
```

-----

##### Navigating to a Different Place

We can also maneuver around the file system `cd`, abbreviated from "change directory".

> `cd Applications`

*This is the same file system as exists when you move around amongst your files in the Finder or Explorer.* The only difference is that we are seeing the contents in text, rather than visually. Using `cd` is the same as double-clicking on a folder to move around and see a new set of files.

Typing the first few letters of where you want to go is usually enough, as the `tab` key will autocomplete. Note that Bash is always *case-sensitive*.

`cd De` will autocomplete to `cd Desktop/` if you hit `tab`. You need to include the lowercase 'e' to disambiguate, as just capital letter 'D' would match `Desktop`, `Documents`, and `Downloads`.

Your prompt will update.

```
ComputerName:Desktop user$
```

And then you can see the contents of your [presumably messy] Desktop with `ls`.

> `ls`

```
Untitled1.ai
Untitled1_Final.ai
Untitled1_Final_FINAL.ai
Untitled1_REALLY_FINAL.ai
Untitled3.ai
```

You can also `cd ..` to move up in the directory structure to return to your home folder. `..` will always move your Terminal's location "up" in the folder hierarchy.

You can also use the `~` character to represent the home folder at any moment. For example, you can always easily return to your home folder.

> `cd ~`

You can also navigate to the root of your computer's file system.

> `cd /`.

And, you can make multiple of jumps at once through your file system.

> `cd /Applications/Utilities/`

-----

##### Redirecting Output

Use the `>` character to redirect output. 

> `echo 'hello zach' > greetings.txt`

Unlike what you would expect, `echo` does not parrot back what you typed in this instance. Instead, the text is pushed into the newly created (if it didn't already exist) `greetings.txt` file. You can preview the new file with the `cat` command, short for `concatenate` since the command is also used for gluing text together, in addition to viewing text files.

> `cat greetings.txt`

```
hello zach
```

Note that we didn't need to create `greetings.txt`. The redirect character `>` checks if a file exists, creates it if it doesn't, and then *replaces* its contents with whatever preceded it. It will always delete whatever is in the file, beware!

> `echo 'buongiorno' > greetings.txt`

> `cat greetings.txt`

```
buongiorno
```

If we want to add text to a file rather than replace its content, we can use `>>`.

> `echo 'kalimera' >> greetings.txt`

> `echo 'merhaba' >> greetings.txt`

> `echo 'hujambo' >> greetings.txt`

> `echo 'rimaykullayki' >> greetings.txt`

> `echo 'ohayo gozaimasu' >> greetings.txt`

> `cat greetings.txt`

```
buongiorno
kalimera
merhaba
hujambo
rimaykullayki
ohayo gozaimasu
```
