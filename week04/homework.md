##### Week 04 Contents
- Presentation: [Electrical Signalling, Inclusive Carshare+Design Principles](readme.md)
- Components: [Multi LED Circuit](circuits.md)
- Homework Review: [Divvy API Access Code](homework-answers.md)
- Code: [Python GPIO Control](python-gpio.md)
- Homework: TBD Based on Class Progress

-----

### Homework for September 17

##### Programming Practice (1 hours)

Review [list and dictionary operations](python-lists.md).

-----

##### Readings (none if you're caught up!)

- We've not spent any classtime on readings, and we'll need to catch up next week! Make sure you've worked through the readings from weeks [2](../week02/homework.md) and [3](../week03/homework.md)  and come prepared to discuss.

-----

##### Application: Transit Availability Visualizer (2-3 hours)

FYI: `pddev` now has internet access! You can do all your data access stuff on your laptops, but code referencing the `GPIO` library will only run on your Raspberry Pi. Leave out that code till the last step.

Wire up the 3 led circuit as indicated on the [Multi LED Circuit Page](circuits.md) and control it with the code we've been building up over the previous weeks.

Let's build a custom transit alarm that gives a user contextual information about the city's transit infrastructure near them. Each of the three LEDs, updating every **2 seconds**, should turn on when one of the following conditions is met...

1. A *train is approaching* a certain stop nearby from one direction
2. A *train is approaching* a certain stop nearby from the other direction
3. A Divvy bicycle *is available* at a nearby dock

As a place to start, your code might be structured like this *pseudo-code*:

- Import libraries
- Set GPIO board mode with `GPIO.setmode` (comment this out on a laptop)
- Make sure all GPIO pins have the correct orientation `GPIO.setup` (comment this out on a laptop)
- Setup variables to hold CTA and Divvy codes for stations of interest (feel free to just look these up!)
- Setup all necessary API variables and URL strings for CTA Train Tracker (maybe use `stpid` to more easily access the separate train directions?)
- Setup all necessary API variables and URL strings for Divvy API
- In a `try :` block to make sure we're safe...
	- In a 	`while True :` infinite loop...
		- Use `requests` to get data from CTA and Divvy
		- Parse data as `json`
		- Remove unnecessary data and structure
		- Check with an `if` condition for each of the 3 states of interest listed above
			- If true, turn on an LED with `GPIO.output`! (or if running on laptop, print "train incoming!" or "bike available!")
			- If false, turn off an LED with `GPIO.output`! (or if running on laptop, print "no train" or "no bike")
		- `sleep` the running code to prevent overflowing your CTA key's data allotment
- In an `except :` block...
	`GPIO.cleanup()` in case something went wrong (comment this out on a laptop)


Totally valueless bonus points for our totally non-existant grading schema!

- Could you add a configurable timer so that the train LEDs are illuminated a certain number of minutes *before* the train arrives? This would make our tool much more useful, as we could it would light up when we need to leave to catch the train. Similarly, could we light up the Divvy LED if and only if a configurable number of bikes were available? There is nothing worse when I'm in a rush than watching the last Divvy bicycle roll away from me!

