##### Week 05 Contents
- Presentation: [Inclusive Bus Stop Principles and Design, PWM Signalling](readme.md)
- Components: [RGB Color Control Circuit](circuits.md)
- Code: [Python PWM Control](python-gpio.md)
- Homework Review: [Transit Availability Visualizer](homework-answers.md)
- Homework: [Transit Availability Visualizer Add-Ons](homework.md)

-----

### Homework for September 24

##### Readings (1 hour)

These will seem divergent from our arc so far, but they are whitepapers that strive to use data to model transit accessibility (but with a very different definition of 'accessibility' than we've been orbiting). Many of our next steps will be located in a model-building space as we attempt to construct a definitional *formula for accessible transit* and what its inputs might be.

- [Modeling spatial accessibility to parks: a national study](https://ij-healthgeographics.biomedcentral.com/articles/10.1186/1476-072X-10-31)
- [Developing a common narrative on urban accessibility: An urban planning perspective](https://www.brookings.edu/research/developing-common-narrative-urban-accessibility-planning/)
- [Modeling Transport Accessibility with Open Data: Case Study of St. Petersburg](https://www.sciencedirect.com/science/article/pii/S1877050916326916) (click on "Download full text in PDF")
- [Understanding Transportation Accessibility of Metropolitan Chicago Through Interactive Visualization](https://www.evl.uic.edu/documents/yin_chicago_urbangis2015.pdf)

Please do not *read these papers*, but skim them and try to determine exactly what definitions they are challenging, and what their recommended remediative actions might be.

-----

##### Application: Figure out a few new APIs and add them to our Transit Health Visualizer (2-3 hours)

Take a look at this [CTA Bus Tracker page](https://www.transitchicago.com/developers/bustracker/), and its [linked PDF documentation](https://www.transitchicago.com/assets/1/6/cta_Bus_Tracker_API_Developer_Guide_and_Documentation_20160929.pdf).  

Grab a new API Key (so silly that the city has two different key servers!), skim the documentation, and see what data you can get out of it! It's significantly more powerful than APIs we've encountered previously. As a place to start, could you construct an API request to answer the following flavors of questions?

1. Given a bus stop, could you determine in code when the next buses might arrive, and where they are headed?
2. Given a bus id number, could you follow it around the city and time its daily route?
3. Given a general locational radius, could you find which bus routes pass through?
4. Given specific geographic source and target areas, could you compare which buses a pedestrian could try to catch to get where they want to go?
5. Given a bus route number, could you collect latitude and longitude coordinates of its future and past stops to reconstruct its movement on a map?

Afterwards, integrate data from the the Bus Tracker API into your Transit Visualizer from last week, and swap out the three single color LEDs for your RGB LED. 

Get predictions for the nearest train station, bus station, and Divvy dock. Then, develop a *language of color* to communicate the transit happenings contextual to your living place. Use this visualization logic as a starting point, and feel free to adjust the colors, directions, timings, and conditions to fit your transit environment. All of these colors can be created with standard [`GPIO.output()`](https://github.com/zachpino/digidev-f19/blob/master/week04/python-gpio.md) commands without the need for this week's [PWM](https://github.com/zachpino/digidev-f19/blob/master/week05/python-gpio.md) control. 

- Red: A train towards the Loop is arriving next and within 5 minutes
- Blue: Trains are arriving from both directions within 5 minutes
- Green: A train away from the Loop is arriving next and within 5 minutes
- Magenta (R+B): A bus towards the Loop is arriving next and within 5 minutes
- Yellow (G+R): A bus away from the Loop is arriving next and within 5 minutes
- Cyan (B+G): Trains and buses are all farther than 5 minutes away, but a Divvy Bike is a available
- White (R+B+G): Trains and buses are arriving from all directions within 5 minutes, and bikes are available
- Black (No Light): Trains and buses are far away, and a bike is not available

Bonus: Could you use PWM 

-----

##### Finish Transit Stop Exercise (2-3 hours)

Finish preparing your design guidelines and recommendations document for sharing next week.
