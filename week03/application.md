##### Week 03 Contents
- Presentation: [Inclusive Design Small Group Chats, Electrical Signalling](readme.md)
- Components: [RGB LED Color Controller](circuits.md)
- Code: [Python Lists and Dictionaries](python-lists.md)
- Code: [Python GPIO Control](python-gpio.md)
- Application: [Train Arrival Warning Alarm](application.md)
- Homework: [Readings, Other APIs, Programming Practice](homework.md)
	
-----

### Application

#### Train Arrival Warning Alarm

![train tracker](https://www.transitchicago.com/assets/1/6/pageheader_traintrackersignred.jpg)

Using the [CTA train tracker API](https://www.transitchicago.com/assets/1/6/cta_Train_Tracker_API_Developer_Guide_and_Documentation.pdf), as well as some useful Python modules such as [datetime](https://docs.python.org/2/library/datetime.html) and [requests](https://pypi.org/project/requests/2.7.0/), let's build a tool for alerting observers when trains are approaching a target station.

##### Example with Single Map ID 

```python
import requests
import json
from datetime import datetime
import time

#important API variables
key = ""
stationCode = 41120
count = 5

#construct API URL
url = "http://lapi.transitchicago.com/api/1.0/ttarrivals.aspx?key=" + key + "&max="+ str(count) + "&outputType=JSON&mapid=" + str(stationCode)

#loop forever
while True : 
	#ask API for data
	response = requests.post(url)

	#parse data as json
	trainData = response.json()

	#strip away some unnecessary data 
	incomingTrains = trainData["ctatt"]["eta"]
	
	#see results
	#print(incomingTrains)

	#record current moment
	now = datetime.now()

	#loop through approaching trains
	for i in range ( len (incomingTrains) ):
		#grab the important bit of data in the "arrT" key
		arrivalDateTime = incomingTrains[i]["arrT"]
		#create a time object so we can do time math
		timeObject = datetime.strptime(arrivalDateTime, "%Y-%m-%dT%H:%M:%S")
		#determine how much time is left until train comes
		timeDifference = timeObject - now
		#print feedback
		print("Train towards " + incomingTrains[i]["destNm"] + " in " + str(timeDifference.seconds) + " seconds at " + str(timeObject.time()))

	#delay a bit
	time.sleep(5)

```



##### Example with Multiple Stop IDs

```python
import requests
import json
from datetime import datetime
import time

#important API variables
key = ""
stationCodes = [30213, 30214]
count = 1


#loop forever
while True : 

	#loop through station code list
	for i in range(len(stationCodes)):
		
		#construct API URL
		url = "http://lapi.transitchicago.com/api/1.0/ttarrivals.aspx?key=" + key + "&max="+ str(count) + "&outputType=JSON&stpid=" + str(stationCodes[i])

		#ask API for data
		response = requests.post(url)

		#parse data as json
		trainData = response.json()
		
		#strip away some unnecessary data 
		incomingTrains = trainData["ctatt"]["eta"]
		
		#see results
		#print(incomingTrains)

		#record current moment
		now = datetime.now()

		#grab the important bit of data in the "arrT" key
		arrivalDateTime = incomingTrains[0]["arrT"]

		#create a time object so we can do time math
		timeObject = datetime.strptime(arrivalDateTime, "%Y-%m-%dT%H:%M:%S")

		#determine how much time is left until train comes
		timeDifference = timeObject - now

		#print feedback
		print("Train towards " + incomingTrains[0]["destNm"] + " in " + str(timeDifference.seconds) + " seconds at " + str(timeObject.time()))

	#delay a bit
	time.sleep(5)