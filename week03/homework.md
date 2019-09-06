##### Week 03 Contents
- Presentation: [Inclusive Design Small Group Chats, Electrical Signalling](readme.md)
- Code: [Python Lists and Dictionaries](python-lists.md)
- Application: [Train Arrival Warning Alarm](application.md)
- Homework: [Readings, Other APIs, Programming Practice](homework.md)

-----

### Homework for September 10

##### Programming Practice (2 hours)

Review [list and dictionary operations](python-lists.md).

Complete exercises 7, 10, 13, and 14 at [Practice Python](https://www.practicepython.org), and come with questions! Answers are on the same page. You will need to do some things we did not cover in class, so give these exercises a try, and we will talk about them next week.

-----

##### Readings (2 hours)

- Read this short CityLab article first [*Why Can't Uber and Lyft Be More Wheelchair-Friendly?*](https://www.citylab.com/transportation/2018/12/ride-hailing-users-disabilitiies-wheelchair-access-uber/577855/). Additionally scan [Uber](https://accessibility.uber.com) and [Lyft's](https://blog.lyft.com/posts/lyfts-commitment-to-accessibility) accessiblity claims. Before continuing to the other readings this week, try to identify exactly where Lyft and Uber identify the limits of their responsibilities with respect to impaired customers and employees. 

- Read [Kat Holmes' *Mismatch*](https://drive.google.com/drive/folders/1lRB-g2c6-mOYRbo-Usb9As9pjDypJPDH?usp=sharing), pages 138 - 151 and prepare for discussion (we're skipping some chapters full of examples — worth reading if you have time!). Notably, the final paragraph on page 142 offers a lovely summation of why inclusive design is natively so hard to learn, teach, and implement. As designers, we often pride ourselves on our ability to engage *uncertainty*, and yet Holmes blames an over-dependency on certainty as a prime motivator for exclusionary thinking and doing. How does this feel? How might our design process adapt to accomodate less certainty?

- We will start class with small group discussion on *teaching inclusive design praxis* and *creating inclusive design guidelines*, so please come prepared to prompt one another.

-----

##### Data Exploration (1.5 hours)

Take a peak at the available [Divvy bikeshare APIs](https://gbfs.divvybikes.com/gbfs/gbfs.json). 

-----

Of particular note is the `station_status` API, which shows the current bike availabilities and station capacities at...

```
https://gbfs.divvybikes.com/gbfs/es/station_status.json
```
Also useful is the `station_information` API, which associates a station id number with its physical human-readable address.

```
https://gbfs.divvybikes.com/gbfs/es/station_information.json
```

-----

No key is needed to access this API, as it is an example of an open API standard called the [Global Bikeshare Feed System (GBFS)](https://github.com/NABSA/gbfs) designed for ingestion by other public system computational tools.

Using Python knowledge built up over the last two weeks, try to write individual Python programs that print answers to the following questions.

1. How many **active bikes** are available in the entire Divvy system?
2. How many **broken ("disabled") bikes** are there in the entire Divvy system?
3. Which stations have **no bikes available**?
4. Which stations are **full of bikes**?

Bonus Harder Questions! 
5. What station(s) has the **most docks** total?
6. What station(s) *that has active docks* has the **fewest docks total**?

-----

As an example, take a look at the code below which demonstrates how to access the API. 

Let's pretend we are trying to answer the questions...

- How many **disabled docks** are there in the entire Divvy system?
- Which stations currently have **no docks available**?

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

#See results
#print(ids)
#ids is a dictionary that looks like this...
#{'43': 'Michigan Ave & Washington St', '48': 'Larrabee St & Kingsbury St', '77': 'Clinton St & Madison St', '81': 'Daley Center Plaza', '100': 'Orleans St & Merchandise Mart Plaza', '181': 'LaSalle St & Illinois St'...}


########## Get Station Current State ##########

#access status API
statusResponse = requests.get(statusUrl, headers=headers)

#parse data as json and throw out unnecessary data
statusData = statusResponse.json()["data"]["stations"]

#see results
#print(statusData)


#create a variable to count the number of disabled docks
disabledDockCount = 0
#create an empty list to hold the stations that have zero docks available
noDocks = []


#loop through bike stations
for i in range ( len (statusData) ):
	# statusData[i] is each station's data

	# capture the number of broken docks in a variable
	disabledDocks = statusData[i]["num_docks_disabled"]
	# add number of broken docks at **this station** to our overall counter
	disabledDockCount += disabledDocks

	#check if the station has zero docks
	if statusData[i]["num_docks_available"] == 0 :
		# capture the station ID number of the zero dock station
		stationID = statusData[i]["station_id"]
		# find the readable address of the zero dock station in our ids dictionary
		stationAddress = ids[stationID]
		#add the zero dock station address to our list
		noDocks.append(stationAddress)

#see results! 				
print("Disabled Dock Count: " + str(disabledDockCount))
print("No Available Docks at : " + str(noDocks))
```

This prints something like this:

```
Disabled Dock Count: 13
No Available Docks at: ['Leavitt St & Archer Ave', 'Sheffield Ave & Kingsbury St', 'Wabash Ave & Cermak Rd', 'Clinton St & Lake St', 'Calumet Ave & 33rd St', 'May St & Cullerton St', 'Mies van der Rohe Way & Chicago Ave', 'Ravenswood Ave & Irving Park Rd', 'Wood St & 35th St', 'Jeffery Blvd & 76th St', 'Millard Ave & 26th St', 'Manor Ave & Leland Ave', 'Spaulding Ave & Division St', 'Phillips Ave & 79th St', 'Fairbanks St & Superior St']
```