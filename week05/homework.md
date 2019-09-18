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

##### Finish Transit Stop Analysis Exercise (2-3 hours)

Finish preparing your design guidelines and recommendations document for sharing next week. Drop these in our class shared Drive please before class next week.

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

Bonus: Could you use [PWM](https://github.com/zachpino/digidev-f19/blob/master/week05/python-gpio.md) signals so that the colors reflect the incoming timings of the various transit options? For example, could the red led get brighter and brighter as the train approaches? Take a look through the code below to see a model for this behavior.

```python
import requests
import json
from datetime import datetime
import time
#import RPi.GPIO as GPIO     

#############################################

#personal API key
key = ""

#you can get a list of route codes at...
# http://www.ctabustracker.com/bustime/api/v2/getroutes?format=json&key=...
route = 3

#if you have a route, you can get the directions a bus travels
#http://www.ctabustracker.com/bustime/api/v2/getdirections?format=json&rt=3&key=...
direction = "Southbound"

#if you have route and direction, you can get stops
#http://www.ctabustracker.com/bustime/api/v2/getstops?format=json&rt=3&dir=Southbound&key=...
stop = 1596

#build API url
url = "http://www.ctabustracker.com/bustime/api/v2/getpredictions?key=" + key + "&rt=" + str(route) + "&stpid=" + str(stop) + "&dir=" + direction + "&format=json"

#############################################

#store pin locations in variables
#rPin = 26
#gPin = 19
#bPin = 13

#use BCM numbering system
#GPIO.setmode(GPIO.BCM)          

#set all needed pins as output
#GPIO.setup(rPin, GPIO.OUT)   
#GPIO.setup(gPin, GPIO.OUT)   
#GPIO.setup(bPin, GPIO.OUT)   

#create a PWM object for each pin, with 100 as its pulse frequency in hertz
#rPWM = GPIO.PWM(rPin, 100)    
#gPWM = GPIO.PWM(gPin, 100)    
#bPWM = GPIO.PWM(bPin, 100)    

#startup the PWM signal at 0 duty load
#rPWM.start(0)
#gPWM.start(0)
#bPWM.start(0)

#############################################

#loop forever
while True : 

	#ask API for data
	response = requests.get(url)

	#parse data as json
	busData = response.json()
	
	#strip away some unnecessary data 
	nextBus = busData["bustime-response"]["prd"][0]
	
	#the api gives a predicted arrival time. there is also a predictive countdown in "prdctdn", but that behaves strangely!
	predictedArrivalTimeString = nextBus["prdtm"]

	#record current moment
	now = datetime.now()
	
	#create a time object so we can do time math
	timeObject = datetime.strptime(predictedArrivalTimeString, "%Y%m%d %H:%M")

	#determine how much time is left until train comes
	timeDifference = timeObject - now

	#number of seconds until predicted bus arrival
	secondsTillArrival = timeDifference.seconds

	#we will glow the green LED brighter as the bus approaches so long as it arriving within 5 minutes
	#check if the bus is nearby
	if secondsTillArrival < 300 :
		#the bus is close! 

		#let's check how close the bus is in our time window as a percentage
		#the "300.0" is to force Python2 and Python3 to do floating point division
		#1.0 is far away, 0.0 is arriving now
		arrivalPercentage = secondsTillArrival / 300.0 

		#we want the opposite though -- closer to 1.0 when the bus is approaching and 0.0 when it is far away
		flippedPercentage = 1 - arrivalPercentage

		#and, we want an integer between 0 and 100, not a floating decimal number between 0 and 1
		pwmSignal = round(flippedPercentage*100)

		#view the results
		print("Bus arriving in " + str(secondsTillArrival) + " seconds")
		print("LED receiving " + str(pwmSignal) + " brightness")
		#make an empty new line
		print("\n")

		#turn this on to control your LED
		#gPWM.ChangeDutyCycle(pwmSignal)

	#delay a bit
	time.sleep(2)
```

