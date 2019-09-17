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
4. Given specific geographic source and target areas, could you compare which bus a pedestrian should try to catch to get where they want to go?
5. Given a bus route number, could you collect latitude and longitude coordinates of its future and past stops to reconstruct its movement on a map?

Afterwards, integrate data from the the Bus Tracker API into your Transit Visualizer from last week, and swap out the single color LEDs for your RGB LED. Develop a language of color to communicate the contextual health of the transit area around your living place.

-----

##### Finish Transit Stop Exercise (2-3 hours)

Finish preparing your design guidelines and recommendations document for presentation next week.
