##### Week 04 Contents
- Presentation: [Electrical Signalling, Inclusive Carshare+Design Principles](readme.md)
- Components: [Multi LED Circuit](circuits.md)
- Homework Review: [Divvy API Access Code](homework-answers.md)
- Code: [Python GPIO Control](python-gpio.md)
- Homework: [Transit Availability Visualizer](homework.md)

-----

All of the answers below will vary depending on when the code is run.

-----

1. How many **active bikes** are available in the entire Divvy system?

	Total Active Bikes (not counting checked-out bikes): 4274

```python
import requests
import json
from datetime import datetime
import time

#construct API URLs
#this API associates a station ID number with its address and longitude, latitude coordinate.
infoUrl = "https://gbfs.divvybikes.com/gbfs/en/station_information.json"
#this API gives the current bike counts at each station, but only identifies each station by an id number.
statusUrl = "https://gbfs.divvybikes.com/gbfs/es/station_status.json"

#accessing the Divvy API requires us to identify our computer as though we were a web browser. Weird. This is called a "header," and is an encoding for how browsers identify themselves.
headers = {'User-Agent': 'Safari/537.36'}

########## Get Station Addresses ##########

#ask information API for data
infoResponse = requests.get(infoUrl, headers=headers)

#parse data as json and access just the data we need
infoData = infoResponse.json()["data"]["stations"]

#create an empty dictionary
ids = {}

#loop through the station information
for i in range ( len (infoData) ):
	
	#save the station id as a variable
	index = (infoData[i]["station_id"])
	
	#create a new key for each station with its id number, and associate its address with that key
	ids[index] = infoData[i]["name"]

########## Get Station Current State ##########

#access status API
statusResponse = requests.get(statusUrl, headers=headers)

#parse data as json and throw out unnecessary data
statusData = statusResponse.json()["data"]["stations"]

#create a variable to count the number of active bikes
activeBikeCount = 0

#loop through bike stations
for i in range ( len (statusData) ):

	# capture the number of active bikes in a variable
	activeBikes = statusData[i]["num_bikes_available"]
	# add number of active bikes at **this station** to our overall counter
	activeBikeCount += activeBikes

#see results! 				
print("Total Active Bikes (not counting checked-out bikes): " + str(activeBikeCount))
```

-----

2. How many **broken ("disabled") bikes** are there in the entire Divvy system?

	Total Disabled Bikes: 218

```python
import requests
import json
from datetime import datetime
import time

#construct API URLs
#this API associates a station ID number with its address and longitude, latitude coordinate.
infoUrl = "https://gbfs.divvybikes.com/gbfs/en/station_information.json"
#this API gives the current bike counts at each station, but only identifies each station by an id number.
statusUrl = "https://gbfs.divvybikes.com/gbfs/es/station_status.json"

#accessing the Divvy API requires us to identify our computer as though we were a web browser. Weird. This is called a "header," and is an encoding for how browsers identify themselves.
headers = {'User-Agent': 'Safari/537.36'}

########## Get Station Addresses ##########

#ask information API for data
infoResponse = requests.get(infoUrl, headers=headers)

#parse data as json and access just the data we need
infoData = infoResponse.json()["data"]["stations"]

#create an empty dictionary
ids = {}

#loop through the station information
for i in range ( len (infoData) ):
	
	#save the station id as a variable
	index = (infoData[i]["station_id"])
	
	#create a new key for each station with its id number, and associate its address with that key
	ids[index] = infoData[i]["name"]

########## Get Station Current State ##########

#access status API
statusResponse = requests.get(statusUrl, headers=headers)

#parse data as json and throw out unnecessary data
statusData = statusResponse.json()["data"]["stations"]

#create a variable to count the number of disabled bikes
disabledBikeCount = 0

#loop through bike stations
for i in range ( len (statusData) ):

	# capture the number of disabled bikes in a variable
	disabledBikes = statusData[i]["num_bikes_disabled"]
	# add number of disabled bikes at **this station** to our overall counter
	disabledBikeCount += disabledBikes

#see results! 				
print("Total Disabled Bikes: " + str(disabledBikeCount))
```

-----

3. Which stations have **no bikes available**?

	No Bikes Available:
	```
	['Racine Ave & 18th St', 'Aberdeen St & Jackson Blvd', 'McClurg Ct & Illinois St', 'Larrabee St & Menomonee St', 'Ashland Ave & Augusta Blvd', 'Marshfield Ave & Cortland St', 'Wabash Ave & Roosevelt Rd', 'Eckhart Park', 'Carpenter St & Huron St', 'Sheffield Ave & Willow St', 'Clark St & Armitage Ave', 'Sedgwick St & North Ave', 'Blackstone Ave & Hyde Park Blvd', 'Damen Ave & Chicago Ave', 'Damen Ave & Division St', 'Clybourn Ave & Division St', 'Clark St & Lincoln Ave', 'Sedgwick St & Webster Ave', 'Southport Ave & Belmont Ave', 'Clark St & Wellington Ave', 'Claremont Ave & Hirsch St', 'Rush St & Cedar St', 'Wells St & Polk St', 'Clark St & Elm St', 'Stave St & Armitage Ave', 'Emerald Ave & 28th St', 'Damen Ave & Grand Ave', 'Halsted St & Willow St', 'Sedgwick St & Schiller St', 'MLK Jr Dr & 29th St', 'Michigan Ave & 18th St', 'Ashland Ave & Grand Ave', 'Larrabee St & Armitage Ave', 'Wells St & Concord Ln', 'Wells St & Evergreen Ave', 'Seeley Ave & Roscoe St', 'Damen Ave & Charleston St', 'Lakeview Ave & Fullerton Pkwy', 'Greenview Ave & Diversey Pkwy', 'Loomis St & Lexington St', 'Stockton Dr & Wrightwood Ave', 'Burling St (Halsted) & Diversey Pkwy (Temp)', 'Ashland Ave & Blackhawk St', 'Emerald Ave & 31st St', 'Racine Ave & Wrightwood Ave', 'Halsted St & Wrightwood Ave', 'Jeffery Blvd & 76th St', 'Shields Ave & 28th Pl', 'Shields Ave & 31st St', 'Ellis Ave & 53rd St', 'California Ave & 23rd Pl', 'Broadway & Thorndale Ave', 'Damen Ave & Foster Ave', 'Throop St & 52nd St', 'Western Blvd & 48th Pl', 'Wood St & Chicago Ave (*)', 'Western Ave & Fillmore St (*)', 'Wood St & Augusta Blvd']
	```

```python
import requests
import json
from datetime import datetime
import time

#construct API URLs
#this API associates a station ID number with its address and longitude, latitude coordinate.
infoUrl = "https://gbfs.divvybikes.com/gbfs/en/station_information.json"
#this API gives the current bike counts at each station, but only identifies each station by an id number.
statusUrl = "https://gbfs.divvybikes.com/gbfs/es/station_status.json"

#accessing the Divvy API requires us to identify our computer as though we were a web browser. Weird. This is called a "header," and is an encoding for how browsers identify themselves.
headers = {'User-Agent': 'Safari/537.36'}

########## Get Station Addresses ##########

#ask information API for data
infoResponse = requests.get(infoUrl, headers=headers)

#parse data as json and access just the data we need
infoData = infoResponse.json()["data"]["stations"]

#create an empty dictionary
ids = {}

#loop through the station information
for i in range ( len (infoData) ):
	
	#save the station id as a variable
	index = (infoData[i]["station_id"])
	
	#create a new key for each station with its id number, and associate its address with that key
	ids[index] = infoData[i]["name"]

########## Get Station Current State ##########

#access status API
statusResponse = requests.get(statusUrl, headers=headers)

#parse data as json and throw out unnecessary data
statusData = statusResponse.json()["data"]["stations"]

#create an empty list to hold the stations that have zero bikes available
noBikes = []

#loop through bike stations
for i in range ( len (statusData) ):

	#check if the station has zero bikes
	if statusData[i]["num_bikes_available"] == 0 :
		# capture the station ID number of the zero bike station
		stationID = statusData[i]["station_id"]
		# find the readable address of the zero bike station in our ids dictionary
		stationAddress = ids[stationID]
		#add the zero bike station address to our list
		noBikes.append(stationAddress)

#see results! 				
print("No Bikes Available: " + str(noBikes))
```

-----

4. Which stations are **full of bikes**?

	Stations full of bikes: 
	```
	['Kimball Ave & Belmont Ave', 'Bosworth Ave & Howard St', 'Leavitt St & Belmont Ave (*)']
	```

```python
import requests
import json
from datetime import datetime
import time

#construct API URLs
#this API associates a station ID number with its address and longitude, latitude coordinate.
infoUrl = "https://gbfs.divvybikes.com/gbfs/en/station_information.json"
#this API gives the current bike counts at each station, but only identifies each station by an id number.
statusUrl = "https://gbfs.divvybikes.com/gbfs/es/station_status.json"

#accessing the Divvy API requires us to identify our computer as though we were a web browser. Weird. This is called a "header," and is an encoding for how browsers identify themselves.
headers = {'User-Agent': 'Safari/537.36'}

########## Get Station Addresses ##########

#ask information API for data
infoResponse = requests.get(infoUrl, headers=headers)

#parse data as json and access just the data we need
infoData = infoResponse.json()["data"]["stations"]

#create an empty dictionary
ids = {}

#loop through the station information
for i in range ( len (infoData) ):
	
	#save the station id as a variable
	index = (infoData[i]["station_id"])
	
	#create a new key for each station with its id number, and associate its address with that key
	ids[index] = infoData[i]["name"]

########## Get Station Current State ##########

#access status API
statusResponse = requests.get(statusUrl, headers=headers)

#parse data as json and throw out unnecessary data
statusData = statusResponse.json()["data"]["stations"]

#create an empty list to hold the stations that are full
fullBikes = []

#loop through bike stations
for i in range ( len (statusData) ):

	#check if the station is full of bikes
	if (statusData[i]["num_bikes_available"] + statusData[i]["num_bikes_disabled"]) == (statusData[i]["num_docks_available"] + statusData[i]["num_docks_disabled"]):
	#could also be with less precision ...
	#if statusData[i]["num_docks_available"] == 0 :

		# capture the station ID number of the full bikes station
		stationID = statusData[i]["station_id"]
		# find the readable address of the full bikes station in our ids dictionary
		stationAddress = ids[stationID]
		#add the full bike station address to our list
		fullBikes.append(stationAddress)

#see results! 				
print("Stations full of bikes: " + str(fullBikes))
```

-----

5. What station(s) has the **most docks** total?

	Lots of ways to do this [more efficiently](https://stackoverflow.com/questions/5320871/in-list-of-dicts-find-min-value-of-a-common-dict-field), but we can use a series of simple list and dictionary manipulations to find the answer.

	```
	Highest dock count:55
	The stations with the highest dock count:['Shedd Aquarium', 'Field Museum']
	``` 

```python
import requests
import json
from datetime import datetime
import time

#construct API URLs
#this API associates a station ID number with its address and longitude, latitude coordinate.
infoUrl = "https://gbfs.divvybikes.com/gbfs/en/station_information.json"
#this API gives the current bike counts at each station, but only identifies each station by an id number.
statusUrl = "https://gbfs.divvybikes.com/gbfs/es/station_status.json"

#accessing the Divvy API requires us to identify our computer as though we were a web browser. Weird. This is called a "header," and is an encoding for how browsers identify themselves.
headers = {'User-Agent': 'Safari/537.36'}

########## Get Station Addresses ##########

#ask information API for data
infoResponse = requests.get(infoUrl, headers=headers)

#parse data as json and access just the data we need
infoData = infoResponse.json()["data"]["stations"]

#create an empty dictionary
ids = {}

#loop through the station information
for i in range ( len (infoData) ):
	
	#save the station id as a variable
	index = (infoData[i]["station_id"])
	
	#create a new key for each station with its id number, and associate its address with that key
	ids[index] = infoData[i]["name"]

########## Get Station Current State ##########

#access status API
statusResponse = requests.get(statusUrl, headers=headers)

#parse data as json and throw out unnecessary data
statusData = statusResponse.json()["data"]["stations"]

#create an empty list to hold the stations that are full
dockCounts = []

#loop through bike stations
for i in range ( len (statusData) ):

	#capture the number of docks and add it to our list
	dockCounts.append( int( statusData[i]["num_docks_available"]) + int(statusData[i]["num_docks_disabled"] ) )

#determine what the highest number of docks is in the list
highestDockCount = max(dockCounts)

#create list of stations that match the highest dock count
highestDockStations = []

#loop through the stations again!
for i in range ( len (statusData) ):

	#check if this station has the same number of docks as our highest dock number
	if (int(statusData[i]["num_docks_available"]) + int( statusData[i]["num_docks_disabled"]) ) == highestDockCount :
		# capture the station ID number of the highest dock station station
		stationID = statusData[i]["station_id"]
		# find the readable address of the highest dock station in our ids dictionary
		stationAddress = ids[stationID]
		#add the highest dock station address to our list
		highestDockStations.append(stationAddress)


#see results! 			
print("Highest dock count:" + str(highestDockCount))	
print("The stations with the highest dock count:" + str(highestDockStations))
```

-----

6. What station(s) *that has active docks* has the **fewest docks total**?

	Same model as question 5, but a conditional included.

	```
	Lowest dock count:6
	The stations with the lowest dock count:['Western Ave & 24th St', 'Damen Ave & 51st St', 'Phillips Ave & 79th St', 'South Chicago Ave & Elliot Ave']
	```

```python
import requests
import json
from datetime import datetime
import time

#construct API URLs
#this API associates a station ID number with its address and longitude, latitude coordinate.
infoUrl = "https://gbfs.divvybikes.com/gbfs/en/station_information.json"
#this API gives the current bike counts at each station, but only identifies each station by an id number.
statusUrl = "https://gbfs.divvybikes.com/gbfs/es/station_status.json"

#accessing the Divvy API requires us to identify our computer as though we were a web browser. Weird. This is called a "header," and is an encoding for how browsers identify themselves.
headers = {'User-Agent': 'Safari/537.36'}

########## Get Station Addresses ##########

#ask information API for data
infoResponse = requests.get(infoUrl, headers=headers)

#parse data as json and access just the data we need
infoData = infoResponse.json()["data"]["stations"]

#create an empty dictionary
ids = {}

#loop through the station information
for i in range ( len (infoData) ):
	
	#save the station id as a variable
	index = (infoData[i]["station_id"])
	
	#create a new key for each station with its id number, and associate its address with that key
	ids[index] = infoData[i]["name"]

########## Get Station Current State ##########

#access status API
statusResponse = requests.get(statusUrl, headers=headers)

#parse data as json and throw out unnecessary data
statusData = statusResponse.json()["data"]["stations"]

#create an empty list to hold the stations that are full
dockCounts = []

#loop through bike stations
for i in range ( len (statusData) ):

	#capture the number of docks and add it to our list
	if ( int( statusData[i]["num_docks_available"]) + int(statusData[i]["num_bikes_available"] ) ) > 0 : 
		dockCounts.append( int( statusData[i]["num_docks_available"]) + int(statusData[i]["num_bikes_available"] ) )

#determine what the highest number of docks is in the list
lowestDockCount = min(dockCounts)

#create list of stations that match the highest dock count
lowestDockStations = []

#loop through the stations again!
for i in range ( len (statusData) ):

	#check if this station has the same number of docks as our lowest dock number
	if ( int(statusData[i]["num_docks_available"]) + int(statusData[i]["num_bikes_available"]) ) == lowestDockCount :
		# capture the station ID number of the highest dock station station
		stationID = statusData[i]["station_id"]
		# find the readable address of the highest dock station in our ids dictionary
		stationAddress = ids[stationID]
		#add the highest dock station address to our list
		lowestDockStations.append(stationAddress)


#see results! 			
print("Lowest dock count:" + str(lowestDockCount))	
print("The stations with the lowest dock count:" + str(lowestDockStations))
```

-----

### The Romano-Ichikawa Approach

A different method to questions 5 and 6 came up in class — which is significantly more efficient (it only needs to loop once!).

```python
	import requests
	import json
	from datetime import datetime
	import time

	#construct API URLs
	#this API associates a station ID number with its address and longitude, latitude coordinate.
	infoUrl = "https://gbfs.divvybikes.com/gbfs/en/station_information.json"
	#this API gives the current bike counts at each station, but only identifies each station by an id number.
	statusUrl = "https://gbfs.divvybikes.com/gbfs/es/station_status.json"

	#accessing the Divvy API requires us to identify our computer as though we were a web browser. Weird. This is called a "header," and is an encoding for how browsers identify themselves.
	headers = {'User-Agent': 'Safari/537.36'}

	########## Get Station Addresses ##########

	#ask information API for data
	infoResponse = requests.get(infoUrl, headers=headers)

	#parse data as json and access just the data we need
	infoData = infoResponse.json()["data"]["stations"]

	#create an empty dictionary
	ids = {}

	#loop through the station information
	for i in range ( len (infoData) ):
		
		#save the station id as a variable
		index = (infoData[i]["station_id"])
		
		#create a new key for each station with its id number, and associate its address with that key
		ids[index] = infoData[i]["name"]

	########## Get Station Current State ##########

	#access status API
	statusResponse = requests.get(statusUrl, headers=headers)

	#parse data as json and throw out unnecessary data
	statusData = statusResponse.json()["data"]["stations"]

	#we will start our search for maximums at zero
	highestDockCount = 0

	#create list of stations that match the highest dock count
	highestDockStations = []

	#loop through the stations again!
	for i in range ( len (statusData) ):

		#check if this station has a larger number of total docks than our previously encountered highest dock number
		#we need to capture the name of the station, delete our previously logged station names, and update counter
		if (int(statusData[i]["num_docks_available"]) + int(statusData[i]["num_bikes_available"])) >  highestDockCount :
			
			#####find the name of the station and add it to the list
			# capture the station ID number of the highest dock station station
			stationID = statusData[i]["station_id"]
			# find the readable address of the highest dock station in our ids dictionary
			stationAddress = ids[stationID]
			#add the highest dock station address to our list
			highestDockStations.append(stationAddress)

			##### wipe our list of previous stations, which had a smaller number of total docks
			del(highestDockStations[:])
			
			##### update the counter to reflect our new max dock number
			highestDockCount = int(statusData[i]["num_docks_available"]) + int(statusData[i]["num_bikes_available"])
		
		#check if this station has the *same* number of docks as our max dock number
		#in this situation, we only need to capture the name of the station
		elif (int(statusData[i]["num_docks_available"]) + int(statusData[i]["num_bikes_available"])) ==  highestDockCount :	
			#####find the name of the station and add it to the list
			# capture the station ID number of the highest dock station station
			stationID = statusData[i]["station_id"]
			# find the readable address of the highest dock station in our ids dictionary
			stationAddress = ids[stationID]
			#add the highest dock station address to our list
			highestDockStations.append(stationAddress)


	#see results! 			
	print("Highest dock count:" + str(highestDockCount))	
	print("The stations with the highest dock count:" + str(highestDockStations))
```

-----

### The Jin Approach

Another additional method to questions 5 and 6 was also mentioned in passing — which is the absolute most efficient method (it harnesses Python's multithreading abilities) using [lambda and filter expressions](https://stackoverflow.com/questions/5320871/in-list-of-dicts-find-min-value-of-a-common-dict-field)).

These are hard to understand, but super fast! We'll talk about these in future weeks.

	```
	Highest dock count:55
	The stations with the highest dock count:['Field Museum', 'Columbus Dr & Randolph St']
	```

```python
import requests
import json
from datetime import datetime
import time

#construct API URLs
#this API associates a station ID number with its address and longitude, latitude coordinate.
infoUrl = "https://gbfs.divvybikes.com/gbfs/en/station_information.json"
#this API gives the current bike counts at each station, but only identifies each station by an id number.
statusUrl = "https://gbfs.divvybikes.com/gbfs/es/station_status.json"

#accessing the Divvy API requires us to identify our computer as though we were a web browser. Weird. This is called a "header," and is an encoding for how browsers identify themselves.
headers = {'User-Agent': 'Safari/537.36'}

########## Get Station Addresses ##########

#ask information API for data
infoResponse = requests.get(infoUrl, headers=headers)

#parse data as json and access just the data we need
infoData = infoResponse.json()["data"]["stations"]

#create an empty dictionary
ids = {}

#loop through the station information
for i in range ( len (infoData) ):
	
	#save the station id as a variable
	index = (infoData[i]["station_id"])
	
	#create a new key for each station with its id number, and associate its address with that key
	ids[index] = infoData[i]["name"]

########## Get Station Current State ##########

#access status API
statusResponse = requests.get(statusUrl, headers=headers)

#parse data as json and throw out unnecessary data
statusData = statusResponse.json()["data"]["stations"]

#get the single highest station dictionary
singleHighestDock = max(statusData, key=lambda station: int(station["num_docks_available"]) + int(station["num_bikes_available"]) )

#get the actual numbers of docks
highestDockCount = int(singleHighestDock["num_docks_available"]) + int(singleHighestDock["num_bikes_available"])

#create list of stations that match the highest dock count by filtering the list of station data based on the discovered max adn converting the resulting *filter* object into a list.
highestDockStationDicts = list(filter(lambda station: (int(station["num_docks_available"]) + int(station["num_bikes_available"]) == highestDockCount), statusData))

#list to hold names of high dock stations
highestDockStations = []

#loop through the stations that matched our discovered highest dock count...
for i in range ( len (highestDockStationDicts) ):

	#####find the name of the station and add it to the list
	# capture the station ID number of the highest dock station station
	stationID = highestDockStationDicts[i]["station_id"]
	# find the readable address of the highest dock station in our ids dictionary
	stationAddress = ids[stationID]
	#add the highest dock station address to our list
	highestDockStations.append(stationAddress)

#see results! 			
print("Highest dock count:" + str(highestDockCount))	
print("The stations with the highest dock count:" + str(highestDockStations))

```
