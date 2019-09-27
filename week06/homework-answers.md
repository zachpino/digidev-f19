##### Week 06 Contents
- Presentation: [Everyone's Research on Station Accessibility](readme.md)
- Code: [Python Data Plotting with MatPlotLib](python-plotting.md)
- Homework Review: [Transit Availability Visualizer with Add-Ons](homework-answers.md)
- Homework: [Readings, Plotting](homework.md)

-----

##### Homework Review

Again, one approach among many possibilities, many of which are more efficient than this one. But, this is understandable and has a clear rhythm. 

Note the commented lines, which if uncommented, would allow this code to run on a Raspberry Pi and light up LEDs as explained in the homework description.

```python
#####################################################################################
# Necessary Python Modules
#####################################################################################

import requests
import json
import time  
from datetime import datetime
#import RPi.GPIO as GPIO


#####################################################################################
# LED Setup
#####################################################################################

#hardware variables for pins
#rLed = 26
#gLed = 19
#bLed = 13

#set our board mode, and disable useless warnings.
#GPIO.setwarnings(False)
#GPIO.setmode(GPIO.BCM)     

#set pin direction
# GPIO.setup(rLed, GPIO.OUT)   
# GPIO.setup(gLed, GPIO.OUT)   
# GPIO.setup(bLed, GPIO.OUT)   

#####################################################################################
# CTA Train Tracker API
#####################################################################################

#CTA Train API Key
trainKey = ""

#CTA API variables
#30381 is Green Line Cermak-Chinatown towards Harlem
#30382 is Green Line Cermak-Chinatown towards 63rd
IBtrainCode = 30381
OBtrainCode = 30382

#construct API URLs for inbound and outbound CTA trains
IBtrainUrl = "http://lapi.transitchicago.com/api/1.0/ttarrivals.aspx?key=" + trainKey + "&max=1&outputType=JSON&stpid=" + str(IBtrainCode)
OBtrainUrl = "http://lapi.transitchicago.com/api/1.0/ttarrivals.aspx?key=" + trainKey + "&max=1&outputType=JSON&stpid=" + str(OBtrainCode)

#####################################################################################
# Divvy API
#####################################################################################

#273 is the code for the station at 18th and Michigan, as extracted from the info API
DVstationCode = 273

#accessing the Divvy API requires us to identify our computer as though we were a web browser. 
headers = {'User-Agent': 'Safari/537.36'}

#API URL for Divvy station current status
DVurl = "https://gbfs.divvybikes.com/gbfs/en/station_status.json"

#####################################################################################
# CTA Bus Tracker API
#####################################################################################

#CTA Bus API Key
busKey = ""

#Bus Route 3 is the King Drive Bus that travels up and down Michigan and MLK Drive
busRoute = 3

#It travels in all 4 directions, but the CTA considers is a mostly north-south bus line
IBbusDirection = "Northbound"
OBbusDirection = "Southbound"

#These are the stops on either side of the Michigan and 18th Street intersection
IBbusCode = 1575
OBbusCode = 1596

#construct API URLs for inbound and outbound CTA buses
IBbusUrl = "http://www.ctabustracker.com/bustime/api/v2/getpredictions?key=" + busKey + "&rt=" + str(busRoute) + "&stpid=" + str(IBbusCode) + "&dir=" + IBbusDirection + "&format=json"
OBbusUrl = "http://www.ctabustracker.com/bustime/api/v2/getpredictions?key=" + busKey + "&rt=" + str(busRoute) + "&stpid=" + str(OBbusCode) + "&dir=" + OBbusDirection + "&format=json"

#####################################################################################
# Main Code Loop
#####################################################################################

#protect our code from errors blowing things up
try :
	#loop forever
	while True : 

#####################################################################################
# Get Data from APIs
#####################################################################################

		#ask CTA Train Tracker API for data
		IBtrainResponse = requests.get(IBtrainUrl)
		OBtrainResponse = requests.get(OBtrainUrl)

		#ask Divvy API for data
		DVresponse = requests.get(DVurl, headers=headers)

		#ask CTA Bus Tracker API for data
		IBbusResponse = requests.get(IBbusUrl)
		OBbusResponse = requests.get(OBbusUrl)

#####################################################################################
# Parse and Filter Data
#####################################################################################

		#parse train data as json and grab only meaningful bits
		#the [0] index is used to get the only the nearest train
		IBtrainData = IBtrainResponse.json()["ctatt"]["eta"][0]
		OBtrainData = OBtrainResponse.json()["ctatt"]["eta"][0]

		# #parse Divvy data as json and access just the data we need
		DVdata = DVresponse.json()["data"]["stations"]
	
		#parse bus data as json and grab only meaningful bits
		#the [0] index is used to get the only the nearest bus
		IBbusData = IBbusResponse.json()["bustime-response"]["prd"][0]
		OBbusData = OBbusResponse.json()["bustime-response"]["prd"][0]


#####################################################################################
# Grab Predicted Arrival Times and Convert to Time Objects for Buses and Trains
#####################################################################################

		#extract train predicted times
		IBtrainPredictedTimeRaw = IBtrainData["arrT"]
		OBtrainPredictedTimeRaw = OBtrainData["arrT"]
		
		#convert predicted train strings to more useful time objects
		IBtrainPredictedTimeObject = datetime.strptime(IBtrainPredictedTimeRaw, "%Y-%m-%dT%H:%M:%S")
		OBtrainPredictedTimeObject = datetime.strptime(OBtrainPredictedTimeRaw, "%Y-%m-%dT%H:%M:%S")


		#extract bus predicted times
		IBbusPredictedTimeRaw = IBbusData["prdtm"]
		OBbusPredictedTimeRaw = OBbusData["prdtm"]

		#convert predicted bus strings to more useful time objects
		IBbusPredictedTimeObject = datetime.strptime(IBbusPredictedTimeRaw, "%Y%m%d %H:%M")
		OBbusPredictedTimeObject = datetime.strptime(OBbusPredictedTimeRaw, "%Y%m%d %H:%M")

#####################################################################################
# Get Current Time
#####################################################################################
		
		now = datetime.now()

#####################################################################################
# Do Time Math to get Seconds till Arrival for Buses and Trains
#####################################################################################

		#get the number of seconds imto that future by subtracting the current time from future arrival time
		IBtrainSeconds = (IBtrainPredictedTimeObject - now).seconds
		OBtrainSeconds = (OBtrainPredictedTimeObject - now).seconds

		IBbusSeconds = (IBbusPredictedTimeObject - now).seconds
		OBbusSeconds = (OBbusPredictedTimeObject - now).seconds

#####################################################################################
# Find Data about Divvy Station of Interest and Extract Numebr of Bikes Available
#####################################################################################

		#loop through all Divvy Station
		for i in range(len(DVdata)) :
			#check if this station is the station of interest
			if int(DVdata[i]["station_id"]) == DVstationCode :
				
				#store number of bikes available in variable
				DVbikesAvailable = int(DVdata[i]["num_bikes_available"])
				
				#this stops the loop once we get a match to save some time.
				break

#####################################################################################
# Preparatory Comparisons for Red, Green, Magenta, and Yellow Behaviors
#####################################################################################

		#we will build a list for comparison
		#if index 0 is lowest, inbound train is next; if 1 is lowest, outbound train is lowest...
		arrivalSeconds = [ IBtrainSeconds, OBtrainSeconds, IBbusSeconds, OBbusSeconds ]

		#find lowest number in list of arrival seconds
		nearestArrival = min(arrivalSeconds)

		#look up the lowest value to find its index
		nearestIndex = arrivalSeconds.index(nearestArrival)

#####################################################################################
# Check for White Light
#####################################################################################
		
		if (IBtrainSeconds < 300) and (OBtrainSeconds < 300) and (IBbusSeconds < 300) and (OBbusSeconds < 300) and (DVbikesAvailable > 0) :
			print("White Light!")
			#GPIO.output(rLed, GPIO.HIGH) 
			#GPIO.output(gLed, GPIO.HIGH) 
			#GPIO.output(bLed, GPIO.HIGH) 

#####################################################################################
# Check for Blue Light
#####################################################################################

		elif (IBtrainSeconds < 300) and (OBtrainSeconds < 300) : 
			print("Blue Light!")
			#GPIO.output(rLed, GPIO.LOW) 
			#GPIO.output(gLed, GPIO.LOW) 
			#GPIO.output(bLed, GPIO.HIGH) 

#####################################################################################
# Check for Red, Green, Magenta, and Yellow Lights
#####################################################################################

		#using the comparisons completed above, 
		elif (arrivalSeconds[nearestIndex] < 300) and (nearestIndex == 0) : 
			print("Red Light! Inbound Train Incoming!")
			#GPIO.output(rLed, GPIO.HIGH) 
			#GPIO.output(gLed, GPIO.LOW) 
			#GPIO.output(bLed, GPIO.LOW) 

		elif (arrivalSeconds[nearestIndex] < 300) and (nearestIndex == 1) : 
			print("Green Light! Outbound Train Incoming!")
			#GPIO.output(rLed, GPIO.LOW) 
			#GPIO.output(gLed, GPIO.HIGH) 
			#GPIO.output(bLed, GPIO.LOW) 

		elif (arrivalSeconds[nearestIndex] < 300) and (nearestIndex == 2) : 
			print("Magenta Light! Inbound Bus Incoming!")
			#GPIO.output(rLed, GPIO.HIGH) 
			#GPIO.output(gLed, GPIO.LOW) 
			#GPIO.output(bLed, GPIO.HIGH) 

		elif (arrivalSeconds[nearestIndex] < 300) and (nearestIndex == 3) : 
			print("Yellow Light! Outbound Bus Incoming!")
			#GPIO.output(rLed, GPIO.HIGH) 
			#GPIO.output(gLed, GPIO.HIGH) 
			#GPIO.output(bLed, GPIO.LOW)

#####################################################################################
# Check for Cyan Light
#####################################################################################
		
		elif (DVbikesAvailable > 0) : 
			print("Cyan Light! Grab a Bike!")
			#GPIO.output(rLed, GPIO.LOW) 
			#GPIO.output(gLed, GPIO.HIGH) 
			#GPIO.output(bLed, GPIO.HIGH)

#####################################################################################
# Fallback to Black Light
#####################################################################################

		else:
			print("Black Light!")
			#GPIO.output(rLed, GPIO.LOW) 
			#GPIO.output(gLed, GPIO.LOW) 
			#GPIO.output(bLed, GPIO.LOW)

#####################################################################################
# Feedback Information Regardless of Light Choice
#####################################################################################

		#Print all processed data as formatted string
		print("IB Train: " + str(IBtrainSeconds) + "\n" + "OB Train: " + str(OBtrainSeconds) + "\n" + "IB Bus: " + str(IBbusSeconds) + "\n" + "OB Bus: " + str(OBbusSeconds) + "\n" + "Bikes: " + str(DVbikesAvailable) )
		
		#make a blank space in terminal
		print("\n\n")

		#wait a few seconds to honor API limitations
		time.sleep(3)

#####################################################################################
# Fallback in case of Error or Controlled User Quitting
#####################################################################################

#if we quit the Python program with ctrl+c...
except KeyboardInterrupt:
	print("Quitting...")
	# safely reset all pins
	#GPIO.cleanup()  


```

-----

The homework last week also had some prompts about untangling if one could use the CTA Bus Tracker API to answer certain sorts of questions.

1. Given a bus stop, could you determine in code when the next buses might arrive, and where they are headed?
	
	The powerful *Predictions* API is designed for this! You can get predictions for up to 10 bus stops at once. Note that you need to specify bus route direction in your API query.

	```
	http://www.ctabustracker.com/bustime/api/v2/getpredictions?rt=3&direction=Southbound&stpid=1596,1598&key=
	```

-----

2. Given a bus id number, could you follow it around the city and time its daily route?

	The *Vehicles* API provides a list of all buses currently running a specific route, as well as their locations. Note that it is possible that a bus vehicle would be considered on a route, even though it might be waiting *several hours* before it departs or advances to its next route assignment.

	```
	http://www.ctabustracker.com/bustime/api/v2/getvehicles?rt=3&key=
	```

	But, unfortunately, in what seems like particularly bad planning, the API does not associate a moving vehicle with its next upcoming stop or previous stop. So, we could instead use the prediction time API above to continually track a single bus as it advances from stop to stop, as the get prediction API offers a key for `vid` — a specific vehicle identifier.

-----

3. Given a general locational radius, could you find which bus routes pass through?

	There are at least two ways to answer this, depending on how much [*well, ackshually*](https://knowyourmeme.com/memes/ackchyually) one wants to be. 

	**Approach A**: A straightforward and easy-to-code interpretation understands the question as *is there a bus stop for a given line within a radius of a target location?*

	Using a [derivative of the Pythagorean theorem](https://www.purplemath.com/modules/distform.htm), we can calculate the distance from the source location to all of the bus stops, and compare that to our measuring radius. If the distance is smaller than the radius, then the bus stop — and therefore the route — is within range. Easy!

	We could also implement a [Manhattan Distance](https://en.wikipedia.org/wiki/Taxicab_geometry) version of the distance equation, as detailed in the comments.

	```python
	#####################################################################################
	# Necessary Python Modules
	#####################################################################################

	import requests
	import json
	import time  
	from datetime import datetime

	#####################################################################################
	# CTA Bus Tracker API
	#####################################################################################

	#CTA Bus API Key
	busKey = "WsNRBxSRFu9zC77ZQAESXtnBY"

	#Bus Route 3 is the King Drive Bus that travels up and down Michigan and MLK Drive. 3 does intersect
	#As a counter example, bus Route 91 on Austin Ave does not intersect

	busRoute = 3
	busDirection = "Southbound"

	#construct API URLs for inbound and outbound CTA buses
	busUrl = "http://www.ctabustracker.com/bustime/api/v2/getstops?key=" + busKey + "&rt=" + str(busRoute) + "&dir=" + busDirection + "&format=json"

	#####################################################################################
	# Geography
	#####################################################################################

	# radius of interest in miles
	radius = 1.5

	# 1 degree of Latitude is ~69 miles
	radiusDegrees = 1.5/69

	# place of interest
	homeLon = -87.62233
	homeLat = 41.85836

	#####################################################################################
	# Main Code Loop
	#####################################################################################

	#ask CTA Bus Tracker API for data
	busResponse = requests.get(busUrl)

	#parse and get meaningful data
	busStops = busResponse.json()["bustime-response"]["stops"] 

	#keep track of if the route passes through the circle
	intersection = False

	#loop through the bus stops, and calculate the distance in degrees longitude between the home point and the stop
	#the distance equation, derived from a^2 + b^2 = c^2, is sqrt((x2 - x1)^2 + (y2 - y1)^2)
	#this formula above calculates *as the bird flies* distance, which is not how we understand distances in cities where we often cannot move diagonally
	#so we could instead use the manhattan distance equation if we wanted to which is ( |x1 - x2| + |y1 - y2| )

	for i in range(len(busStops)) :
		#as the bird flies
		distance = ( ((busStops[i]["lon"] - homeLon)**2) + ((busStops[i]["lat"] - homeLat)**2) )**.5

		#manhattan distance
		#distance = abs(busStops[i]["lon"] - homeLon) + abs(busStops[i]["lat"] - homeLat)
		
		#check if we're close enough...
		if distance < radiusDegrees:
			intersection = True
			#stop looping if we've found a bus stop in the radius of interest
			break


	#bus stop is in the region! 
	if intersection == True : 
		print("bus route " + str(busRoute) + " passes through!")
		
	#so sad...
	if intersection == False : 
		print("bus route " + str(busRoute) + " does not pass through!")

	```

	**Approach B**: An alternative interpretation assumes that the question is asking *does the bus pass through the radius at all?* as it traverses its route legs from stop to stop. This one is much tougher, and effectively impossible to guarantee due to [daily route variability](https://www.transitchicago.com/travel-information/bus-status/) and [geometric logic](https://en.wikipedia.org/wiki/Coastline_paradox). We would need to reconstruct *all* bus routes in *all their directions* as a series of line segments, select an acceptable level [geometric tolerance dilution](https://en.wikipedia.org/wiki/Dilution_of_precision_(navigation)), and [check if any of those segments intersects a circle](http://doswa.com/2009/07/13/circle-segment-intersectioncollision.html) defined by the center point of interest and the question's radius.

	To facilitate the math, it is easier to make use of the awesome [`sympy` module](https://www.sympy.org/en/index.html) for symbolic geometry and arithmetic. In terminal...

	```
	pip install sympy
	```

	The below method generates line segments for the legs of a single route, and compares those with a geometric circle (the 'radius' in the question). If a leg (line segment) intersects the circle, we know that the bus passes through the circle of interest. But, this method will fail if the *entire route* exists *inside* of the circle of interest, which is pretty unlikely. Note that this method is *slow* — taking around 6 seconds to test a single route — and most GPS tools use a much simplified version of SymPy's mathematics -- which can lead to incorrect results ("rerouting...rerouting...rerouting..."). So, to test all 129 CTA routes, in both directions, this method would take around 25 minutes! Google Maps and similar tools are mind-bogglingly complex things that can only deliver their unbelievably quick, transactional experiences through a repertoire of geometric shortcuts, trigonometric simplifications, and mathematical heuristics.

	```python
		#####################################################################################
	# Necessary Python Modules
	#####################################################################################

	import requests
	import json
	import time  
	from datetime import datetime
	import sympy as sp

	#####################################################################################
	# CTA Bus Tracker API
	#####################################################################################

	#CTA Bus API Key
	busKey = "WsNRBxSRFu9zC77ZQAESXtnBY"

	#Bus Route 3 is the King Drive Bus that travels up and down Michigan and MLK Drive. 3 does intersect
	#As a counter example, bus Route 91 on Austin Ave does not intersect

	busRoute = 3
	busDirection = "Southbound"

	#construct API URLs for inbound and outbound CTA buses
	busUrl = "http://www.ctabustracker.com/bustime/api/v2/getstops?key=" + busKey + "&rt=" + str(busRoute) + "&dir=" + busDirection + "&format=json"

	#####################################################################################
	# Geography
	#####################################################################################

	# radius of interest in miles
	radius = 1.5

	# 1 degree of Latitude is ~69 miles
	radiusDegrees = 1.5/69

	# place of interest
	homeCoordinate = (-87.62233, 41.85836)

	# convert to SymPy Circle object
	homeCircle = sp.Circle(homeCoordinate, sp.sympify(str(radiusDegrees), rational=True))

	#####################################################################################
	# Main Code Loop
	#####################################################################################

	#ask CTA Bus Tracker API for data
	busResponse = requests.get(busUrl)

	#parse and get meaningful data
	busStops = busResponse.json()["bustime-response"]["stops"] 

	#list to hold each leg of the route 
	routeLegs = []

	#loop through the list of stops, and construct a line segment for each leg defined by a start and end point
	#so, we use index i as start point and index i+i as end point
	#we have to make sure we end our loop one position before the end of the list
	for i in range(len(busStops) - 1) :
		start = ( busStops[i]["lon"] , busStops[i]["lat"] )
		end = ( busStops[i+1]["lon"] , busStops[i+1]["lat"] )
		#make a SymPy segment object
		routeLegs.append(sp.Segment2D(sp.Point2D(start), sp.Point2D(end)))

	#keep track of if the route passes through the circle
	success = False

	#loop through the newly created legs, and see if there is an intersection with radius of interest
	for i in range(len(routeLegs)) :
		intersections = sp.intersection(homeCircle, routeLegs[i])

		#SymPy would return a list of points of intersection were there to be an intersection, so we know that this is a successful test based on list length
		if len(intersections) > 0 : 
			#feedback
			print("bus route " + str(busRoute) + " passes through!")
			success = True
			#stop the loop early
			break

	#so sad...
	if success == False : 
		print("bus route " + str(busRoute) + " does not pass through!")

	```

-----

4. Given specific geographic source and target areas, could you compare which buses a pedestrian could try to catch to get where they want to go?

	See the example code for question 3, though this time we would need to test **2 different circles** against all the bus routes! Would you be willing to wait almost an hour for Google Maps to offer you *guaranteed accurate and comprehensive* transit recommendations?

-----

5. Given a bus route number, could you collect latitude and longitude coordinates of its future and past stops to reconstruct its movement on a map?

	The *Stops* API provides latitude and longitude coordinates of all stops based on a route number. 

	```
		http://www.ctabustracker.com/bustime/api/v2/getstops?rt=3&key=...
	```

	Though we might know bus stop positions, it is not only difficult to guarantee how the bus might move through those stops sequentially, but also how it moves into and out of those stops. Dynamic way streets, construction, and traffic conditions all impact how a bus travels its route -- and it is possible that day-to-day the route might change. The CTA only guarantees that buses will stop at each named stops, not how the bus will get to those stops. 

	We could use [matplotlib](python-plotting.md) to take the latitude longitude pairs and create a route path. We also could make use of [Folium](https://python-visualization.github.io/folium/quickstart.html), [Plotly](https://plot.ly/python/), [Basemap](https://matplotlib.org/basemap/), [Geopandas](http://geopandas.org/#), [Mapbox](https://www.mapbox.com/), or any other Python geo-plotting software to create more traditional, potentially interactive maps. We'll be covering some of these tools in the coming weeks.
