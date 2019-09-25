##### Week 05 Contents
- Presentation: [Inclusive Bus Stop Principles and Design, PWM Signalling](readme.md)
- Components: [RGB Color Control Circuit](circuits.md)
- Code: [Python PWM Control](python-gpio.md)
- Homework Review: [Transit Availability Visualizer](homework-answers.md)
- Homework: [Transit Availability Visualizer Add-Ons](homework.md)

-----

##### Homework Review

Here is one approach among many possibilities to the LED transit visualizer homework introduced last week.

Note the commented lines, which if uncommented, would allow this code to run on a Raspberry Pi and light up LEDs.

```python
import requests
import json
import time  
#import RPi.GPIO as GPIO

#hardware variables for pins
#DVled = 16
#NBled = 20
#SBled = 21

#set our board mode, and disable useless warnings.
#GPIO.setwarnings(False)
#GPIO.setmode(GPIO.BCM)     

#set pin direction
# GPIO.setup(NBled, GPIO.OUT)   
# GPIO.setup(SBled, GPIO.OUT)   
# GPIO.setup(DVled, GPIO.OUT)   


#important API variables
key = ""

#CTA API variables
NBstationCode = 30213
SBstationCode = 30214
count = 1

#Divvy API variables
DVstationCode = 4

#accessing the Divvy API requires us to identify our computer as though we were a web browser. Weird. This is called a "header," and is an encoding for how browsers identify themselves.
headers = {'User-Agent': 'Safari/537.36'}

#construct API URLs for NB and SB CTA 
NBurl = "http://lapi.transitchicago.com/api/1.0/ttarrivals.aspx?key=" + key + "&max="+ str(count) + "&outputType=JSON&stpid=" + str(NBstationCode)
SBurl = "http://lapi.transitchicago.com/api/1.0/ttarrivals.aspx?key=" + key + "&max="+ str(count) + "&outputType=JSON&stpid=" + str(SBstationCode)

#URL for Divvy
DVurl = "https://gbfs.divvybikes.com/gbfs/es/station_status.json"

	
#protect our code from errors blowing things up
try :
	#loop forever
	while True : 
		#ask CTA API for data
		NBCTAresponse = requests.get(NBurl)
		SBCTAresponse = requests.get(SBurl)

		#ask Divvy API for data
		DVresponse = requests.get(DVurl, headers=headers)

		#parse train data as json
		NBtrainData = NBCTAresponse.json()
		SBtrainData = SBCTAresponse.json()

		#strip away some unnecessary data 
		#the [0] index is used to get the most recent train
		NBincomingTrains = NBtrainData["ctatt"]["eta"][0]
		SBincomingTrains = SBtrainData["ctatt"]["eta"][0]

		#parse Divvy data as json and access just the data we need
		DVdata = DVresponse.json()["data"]["stations"]
		
		#loop through the Divvy data and match our desired station code
		for i in range(len(DVdata)) :
			if int(DVdata[i]["station_id"]) == DVstationCode :
				DVstationData = DVdata[i]
				#this stops the loop once we get a hit to save some time.
				break
		
		#check for approaching train
		if int(NBincomingTrains["isApp"]) == 1:
			print("northbound train approaching: yes")
			#GPIO.output(NBled, GPIO.HIGH) 
		else:
			print("northbound train approaching: no")
			#GPIO.output(NBled, GPIO.LOW) 

		#check for approaching train
		if int(SBincomingTrains["isApp"]) == 1:
			print("southbound train approaching: yes")
			#GPIO.output(SBled, GPIO.HIGH) 
		else:
			print("southbound train approaching: no")
			#GPIO.output(SBled, GPIO.LOW) 

		#check for available bikes
		if DVstationData["num_bikes_available"] > 0 :
			print("bikes available: yes")
			#GPIO.output(DVled, GPIO.HIGH) 
		else :
			print("bikes available: no")
			#GPIO.output(DVled, GPIO.LOW) 

		#make a blank line in terminal
		print("\n")
		
		#wait two seconds
		time.sleep(2)

#if we press ctrl+c, reset all the pins to their default state
except KeyboardInterrupt:
	print("quitting...")
	# safely reset all pins
	#GPIO.cleanup()  
```

-----

##### Bonus Prompts

The answer to the bonus prompt could look like this:

```python
import requests
import json
import time  
from datetime import datetime
#import RPi.GPIO as GPIO

#hardware variables for pins
#DVled = 16
#NBled = 20
#SBled = 21

#set our board mode, and disable useless warnings.
#GPIO.setwarnings(False)
#GPIO.setmode(GPIO.BCM)     

#set pin direction
# GPIO.setup(NBled, GPIO.OUT)   
# GPIO.setup(SBled, GPIO.OUT)   
# GPIO.setup(DVled, GPIO.OUT)   


#important API variables
key = "fcbd890772d1472391248cddaf3b8a23"

#CTA API variables
NBstationCode = 30213
SBstationCode = 30214
count = 1

#Divvy API variables
DVstationCode = 4

#contextual variables
minutesFromTrainStation = 2
safeBikeMinimum = 5

#accessing the Divvy API requires us to identify our computer as though we were a web browser. Weird. This is called a "header," and is an encoding for how browsers identify themselves.
headers = {'User-Agent': 'Safari/537.36'}

#construct API URLs for NB and SB CTA 
NBurl = "http://lapi.transitchicago.com/api/1.0/ttarrivals.aspx?key=" + key + "&max="+ str(count) + "&outputType=JSON&stpid=" + str(NBstationCode)
SBurl = "http://lapi.transitchicago.com/api/1.0/ttarrivals.aspx?key=" + key + "&max="+ str(count) + "&outputType=JSON&stpid=" + str(SBstationCode)

#URL for Divvy
DVurl = "https://gbfs.divvybikes.com/gbfs/es/station_status.json"

	
#protect our code from errors blowing things up
try :
	#loop forever
	while True : 
		#ask CTA API for data
		NBCTAresponse = requests.get(NBurl)
		SBCTAresponse = requests.get(SBurl)

		#ask Divvy API for data
		DVresponse = requests.get(DVurl, headers=headers)

		#parse train data as json
		NBtrainData = NBCTAresponse.json()
		SBtrainData = SBCTAresponse.json()

		#strip away some unnecessary data 
		#the [0] index is used to get the most recent train
		NBincomingTrains = NBtrainData["ctatt"]["eta"][0]
		SBincomingTrains = SBtrainData["ctatt"]["eta"][0]

		#record current time
		now = datetime.now()

		#format predicted arrivals as time objects
		NBtimeObject = datetime.strptime(NBincomingTrains["arrT"], "%Y-%m-%dT%H:%M:%S")
		SBtimeObject = datetime.strptime(SBincomingTrains["arrT"], "%Y-%m-%dT%H:%M:%S")

		#determine how much time is left until train comes in both directions
		NBtimeDifference = NBtimeObject - now
		SBtimeDifference = SBtimeObject - now

		#parse Divvy data as json and access just the data we need
		DVdata = DVresponse.json()["data"]["stations"]
		
		#loop through the Divvy data and match our desired station code
		for i in range(len(DVdata)) :
			if int(DVdata[i]["station_id"]) == DVstationCode :
				DVstationData = DVdata[i]
				#this stops the loop once we get a hit to save some time.
				break
		
		#check for approaching train
		if NBtimeDifference.seconds < (minutesFromTrainStation * 60) :
			print("northbound train approaching in " + str(NBtimeDifference.seconds) )
			#GPIO.output(NBled, GPIO.HIGH) 
		else:
			print("northbound train approaching: no")
			#GPIO.output(NBled, GPIO.LOW) 

		#check for approaching train
		if SBtimeDifference.seconds < (minutesFromTrainStation * 60) :
			print("southbound train approaching in " + str(SBtimeDifference.seconds) )
			#GPIO.output(SBled, GPIO.HIGH) 
		else:
			print("southbound train approaching: no")
			#GPIO.output(SBled, GPIO.LOW) 

		#check for available bikes
		if DVstationData["num_bikes_available"] > safeBikeMinimum :
			print("bikes available: yes")
			#GPIO.output(DVled, GPIO.HIGH) 
		else :
			print("bikes available: no")
			#GPIO.output(DVled, GPIO.LOW) 

		#make a blank line in terminal
		print("\n")
		
		#wait two seconds
		time.sleep(2)

#if we press ctrl+c, reset all the pins to their default state
except KeyboardInterrupt:
	print("quitting...")
	# safely reset all pins
	#GPIO.cleanup()  
