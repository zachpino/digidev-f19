##### Week 15 Contents
- Presentation: [More to Do with RasPi](readme.md)
- Code Update: [Better Sample Code](project-update.md)
- Logistics: [Course Wrap-Up](wrap-up.md)

-----

### Sample Code for Acrylic Data Physicalizations

Thanks so much to Yuta, we have some very useful utilities, sample code, and better Python modules to run on our data objects. The code posted before, which was designed for Python 2, had some incompatibilities with some of the nicer data-processing tools many of you are now using. It also did a lot of processing and data manipulation to generate a graphical map, which is not required since we have LED arrays instead. 

This new approach is **highly recommended**, and will save you all a lot of time in the end, even if it means tweaking some already completed work. 

Note, though, that for this code to work well, all data passed to the LED animation functions need to be between 0 and 1. Percentage, ranks, sequence position, quantized bins are all great. Take a look at lines 206-237 in the sample code below to see how to transform your data into the appropriate shape. 

-----

##### Module Installations

First, install MatPlotLib for Python 3 **on your Raspberry Pi** through SSH or an HDMI connection.

```
sudo apt-get install python3-matplotlib
```

Similarly, install Pandas for Python 3 on your Raspberry Pi.

```
sudo apt-get install python3-pandas
```

Finally, a different library for running the LEDs.

```
sudo pip3 install adafruit-circuitpython-neopixel
```

And reboot!

```
sudo reboot now
```

**To run your code, you will need to type...**

```
sudo python3 filename.py
```

------

##### Sample Code

Please ensure you skim through the code and the comments before running this. There are many settings to calibrate before it will work correctly.

The code has several sections, and the ones that will likely require attention are marked with asterisks here:

```
- 001 - 007 : Python Module Import
- 011 - 028 : * General Setup *
- 032 - 046 : * LED Strip Setup *
- 050 - 055 : * Census Setup *
- 073 - 147 : * Census Data *
- 151 - 190 : * LED Assignments *
- 194 - 202 : * Merge and Crop Data *
- 206 - 237 : * Synthesize New Data *
- 241 - 607 : LED Utilities
- 611 - 621 : * Make Your Own Colormaps * 
- 625 - End : * Run Program *
```

-----

```python
import requests #for web access
import json #for data parsin
import time #for sleeping
import matplotlib.pyplot as plt #for colors
import pandas as pd #for data manipulation
import numpy as np #for data manipulation
import matplotlib.colors as clr #for making colormaps



################################################################################################################
### General Setup ###
################################################################################################################

#is this running on a Raspberry Pi?
#super important question!!!
#True = Raspberry Pi with LEDs
#False = Macbook Pro
rasPi = False

#if you would like to see extra info about LED behavior printed to Terminal...
#set printDebug to True
printDebug = False

#bring in Raspberry Pi Modules
if rasPi == True:
	import neopixel #for LEDs
	import board #for hardware access



################################################################################################################
### LED Strip Setup ###
################################################################################################################

#Number of LEDs
ledCount = 64

#1 is full brightness, 0 is no light
brightness = .2

#create data object to hold settings for LED strips
#D18 means the data is connected to pin 18
#auto-write as false means that we will manually push data to LEDs for more control
if rasPi == True:
	pixels = neopixel.NeoPixel(board.D18, ledCount, auto_write=False)



################################################################################################################
### Census Setup ###
################################################################################################################

#key for accessing census data
censusKey = "49e7309f9174890982e02ed2a436b8e9f9589ec0"



################################################################################################################
### Safety Checks ###
################################################################################################################

if censusKey == "" :
	print("You forgot to set your census key, you will probably get errors")

if ((brightness * .06) * ledCount) >  .95 :
	print("Brightness is very high, are you sure?")
	print("If you are sure that your RasPi can handle it, delete the 'Exception' line under *Safety Checks*")
	raise Exception('Brightness was too high.')



################################################################################################################
### Get Census Data ###
################################################################################################################

#datapoints of interest from census
#B18105_004E = Ambulatory Disability Male 5-17
#B18105_003E = Total Male 5-17
#B18105_007E = Ambulatory Disability Male 18-34
#B18105_006E = Total Male 18-34

#B18105_020E = Ambulatory Disability Female 5-17
#B18105_019E = Total Female 5-17
#B18105_023E = Ambulatory Disability Female 18-34
#B18105_022E = Total Female 5-17
dataPoints = "B18105_004E,B18105_003E,B18105_007E,B18105_006E,B18105_020E,B18105_019E,B18105_023E,B18105_022E"

#construct URL
url = "https://api.census.gov/data/2017/acs/acs5?get=NAME,B01001_001E," + dataPoints + "&for=tract:*&in=state:17&in=county:031&key=" + censusKey

#for debugging census in browser
#print("Put this into Chrome -> " + url)

#request data from census, and convert to json
censusResponse = requests.get(url).json()

#create array to populate with census data
censusDataList = []

#loop through data from census, starting after the header row
for i in range (1, len(censusResponse)) :

	#skip any tract with no population, as they might cause divide-by-zero errors
	if censusResponse[i][1] == "0" :
		#skip!
		pass
	#all the rows that have a population...
	else:
		#create dictionary of all data, raw as well as computed
		#to find index of different values, work with census in your browser!
		censusDataList.append(
			{	
			"name": censusResponse[i][0],
			"totalPopulation": int(censusResponse[i][1]),
			"male517ambDis" : int(censusResponse[i][2]),
			"male517total" : int(censusResponse[i][3]),

			#note the plus 1 below, to ensure we don't get divide by zero errors
			"male517ambDisPercentage" : int(censusResponse[i][2]) / (int(censusResponse[i][3])+1),
			
			"male1834ambDis" : int(censusResponse[i][4]),
			"male1834total" : int(censusResponse[i][5]),
			"male1834ambDisPercentage" : int(censusResponse[i][4]) / (int(censusResponse[i][5])+1),
			
			"female517ambDis" : int(censusResponse[i][6]),
			"female517total" : int(censusResponse[i][7]),
			"female517ambDisPercentage" : int(censusResponse[i][6]) / (int(censusResponse[i][7])+1),
			"female1834ambDis" : int(censusResponse[i][8]),
			"female1834total" : int(censusResponse[i][9]),
			"female1834ambDisPercentage" : int(censusResponse[i][6]) / (int(censusResponse[i][7])+1),

			"both517ambDisPercentage" : ( int(censusResponse[i][2]) + int(censusResponse[i][6]) ) / (( int(censusResponse[i][3]) + int(censusResponse[i][7]) )+1),
			"both1834ambDisPercentage" : ( int(censusResponse[i][4]) + int(censusResponse[i][8]) ) / (( int(censusResponse[i][5]) + int(censusResponse[i][9]) )+1),
			"both534ambDisPercentage" : ( int(censusResponse[i][2]) + int(censusResponse[i][6]) + int(censusResponse[i][4]) + int(censusResponse[i][8]) ) / (( int(censusResponse[i][3]) + int(censusResponse[i][7]) + int(censusResponse[i][5]) + int(censusResponse[i][9]) )+1),
			"state" : int(censusResponse[i][10]),
			"county" : int(censusResponse[i][11]),
			"tractCode" : (censusResponse[i][12])
			}
		)

#convert census data into a pandas data frame, which is like an excel table in code
#it is much more performative and flexible, but awkward to manipulate
censusFrame = pd.DataFrame(censusDataList)

#see requested data
#print(censusFrame.head())



################################################################################################################
### LED Assignment ###
################################################################################################################

#relate between geographic entitites to LED indices 
leds = [
	{"tractCode": "283800", "leds":[0]},
	{"tractCode": "310400", "leds":[1]},
	{"tractCode": "843200", "leds":[2,13,17]},
	{"tractCode": "840200", "leds":[3,12]},
	{"tractCode": "600600", "leds":[4]},
	{"tractCode": "600900", "leds":[5]},
	{"tractCode": "839700", "leds":[6,9]},
	{"tractCode": "839900", "leds":[7,8]},
	{"tractCode": "841900", "leds":[15,16]},
	{"tractCode": "600400", "leds":[11]},
	{"tractCode": "839800", "leds":[10]},
	{"tractCode": "840000", "leds":[21,22,23]},
	{"tractCode": "840100", "leds":[20]},
	{"tractCode": "841100", "leds":[18,19,29,28]},
	{"tractCode": "330200", "leds":[30,31]},
	{"tractCode": "340300", "leds":[27]},
	{"tractCode": "340500", "leds":[25,26]},
	{"tractCode": "340600", "leds":[24]},
	{"tractCode": "330100", "leds":[32,33,45,46,47,48,49,50]},
	{"tractCode": "841000", "leds":[34,35,44,51,60]},
	{"tractCode": "839200", "leds":[36,43]},
	{"tractCode": "350100", "leds":[52,59]},
	{"tractCode": "351000", "leds":[53,58]},
	{"tractCode": "842000", "leds":[37]},
	{"tractCode": "351400", "leds":[38,39]},
	{"tractCode": "839500", "leds":[42]},
	{"tractCode": "839600", "leds":[40,41]},
	{"tractCode": "360200", "leds":[54,55]},
	{"tractCode": "836500", "leds":[56,57]}
]

#convert data into a pandas data frame, which is like an excel table in code
#it is much more performative and flexible, but awkward to manipulate
leds = pd.DataFrame(leds)



################################################################################################################
### Merge and Crop Data ###
################################################################################################################

#join led indices into our census data based on both tables having a 'tractCode' column
allData = censusFrame.merge(leds, left_on='tractCode', right_on='tractCode')

#throw away all census tracts that don't have LED assignments
allData = allData[allData["tractCode"].isin(leds["tractCode"])]



################################################################################################################
### Synthesize New Data ###
################################################################################################################

#all of the data to be passed into colormap LED logic must be in the form of a float between 0 - 1
#so, we can make new columns similar to a percentage, by dividing each data point by the max value of that datapoint
#we can think of these as ranks, and the process is conceptually similar to matplotlib's coloring logic when we draw maps
#the lowest value in the column becomes a very low number close to 0, and the highest number in the column becomes 1
allData["both535rank"] = allData["both534ambDisPercentage"] / allData["both534ambDisPercentage"].max()
allData["female1834ambDisRank"] = allData["female1834ambDis"] / allData["female1834ambDis"].max()

#we can also do noncontinuous, categorical binning which is called *quantizing*
#this divides a column of data into a set of an equal number of data entitites
#if we set bins=4, we would geo entities in the first quartile as 0, the second quartile as 1, the third quarttile as 2, and the fourth quartile as 3
#this is most useful when you want to show shared, similar behaviorss across entities
bins = 4
allData["female517totalBins"] = pd.qcut(allData['female517total'], q=bins, labels=False)
allData["female517totalBins"] = allData["female517totalBins"] / (bins - 1)

#simple thresholds are also possible, in a more compact and performative form than for loops
threshold = 500
#can also set threshold algorithmically, for example checking if we're above or below the average
#threshold = censusFrame['male517total'].mean()

#make a new column called "male517HighLow" that shows 1 if "male517total" >= 500, or 0 if smaller
allData['male517HighLow'] = np.where( allData['male517total'] >= threshold, 1, 0)

#see some sample data in Terminal
#print(allData.head())

#export to csv to see all data in Excel
#allData.to_csv('data.csv')



################################################################################################################
### Functions for Sending Colors to LEDs and Animating ###
################################################################################################################

def showData(dataProperty, colormap, delaySeconds):
	#show feedback
	print("Showing " + dataProperty + " in " + str(colormap) + " for " + str(delaySeconds) + " seconds.")

	#set color map function -- useful for animating LEDs
	if isinstance(colormap, str):
		cmap = plt.cm.get_cmap(colormap)
	else:
		cmap = colormap

	#iterate through geographic entities one at a time
	#i is the index, geo is the dictionary of all the data for each geographic entity
	for i, geo in allData.iterrows():
		# for debugging or running on your Mac...
		if rasPi == False and printDebug == True:
			print("For " + str(geo["tractCode"]) + ", lighting up LEDs: " + str(geo["leds"]))

		#determine visualization color for current tract
		#cmaps take a number between 0 and 1 as input, and return a tuple of 3 values between 0 and 1 as a color representation
		color = cmap( geo[dataProperty] )

		#separate generated color into rgb channels, and manipulate to save power and create necessary 0-255 values
		r = int( (color[0] * 255) * brightness )
		g = int( (color[1] * 255) * brightness )
		b = int( (color[2] * 255) * brightness )

		#iterate through all leds associated with this geography
		for j in range( len( geo["leds"] )):
			#set the pixel for all LEDs per geography to the generated color
			if rasPi == True:
				pixels[geo["leds"][j]] = (r,g,b)
			else:
				pass

	#after all LEDs have been updated, push the data to the actual LED strip
	if rasPi == True:
		pixels.show()

	#wait
	time.sleep(delaySeconds)

def fadeData(dataProperty, colormap, fadeSeconds, holdSeconds, fadeUp=True, fadeDown=True, fadeSteps=20):
	#show feedback	
	print("Fading or pulsing " + dataProperty + " in " + str(colormap) + " over " + str(fadeSeconds) + " seconds.")

	#set color map function -- useful for animating LEDs
	if isinstance(colormap, str):
		cmap = plt.cm.get_cmap(colormap)
	else:
		cmap = colormap

	if fadeUp == True:
		#slowly increase brightness
		for k in range(fadeSteps):
			#iterate through geographic entities one at a time
			#i is the index, geo is the dictionary of all the data for each geographic entity
			for i, geo in allData.iterrows():
				# for debugging or running on your Mac...
				if rasPi == False and printDebug == True:
					print("For " + str(geo["tractCode"]) + ", lighting up LEDs: " + str(geo["leds"]))

				#determine visualization color for current tract
				#cmaps take a number between 0 and 1 as input, and return a tuple of 3 values between 0 and 1 as a color representation
				color = cmap( geo[dataProperty] )

				#separate generated color into rgb channels, and manipulate to save power and create necessary 0-255 values
				r = int( ( (color[0] * 255) * brightness ) * (k/fadeSteps) )
				g = int( ( (color[1] * 255) * brightness ) * (k/fadeSteps) )
				b = int( ( (color[2] * 255) * brightness ) * (k/fadeSteps) )


				#iterate through all leds associated with this geography
				for j in range( len( geo["leds"] )):
					#set the pixel for all LEDs per geography to the generated color
					if rasPi == True:
						pixels[geo["leds"][j]] = (r,g,b)
					else:
						pass

			#after all LEDs have been updated, push the data to the actual LED strip
			if rasPi == True:
				pixels.show()

			#wait
			time.sleep(fadeSeconds / fadeSteps)

	else : 
		showData(dataProperty,colormap,.1)

	time.sleep(holdSeconds)

	if fadeDown == True:
		#slowly decrease brightness
		for k in range(fadeSteps, -1, -1):
			#iterate through geographic entities one at a time
			#i is the index, geo is the dictionary of all the data for each geographic entity
			for i, geo in allData.iterrows():
				# for debugging or running on your Mac...
				if rasPi == False and printDebug == True:
					print("For " + str(geo["tractCode"]) + ", lighting up LEDs: " + str(geo["leds"]))

				#determine visualization color for current tract
				#cmaps take a number between 0 and 1 as input, and return a tuple of 3 values between 0 and 1 as a color representation
				color = cmap( geo[dataProperty] )

				#separate generated color into rgb channels, and manipulate to save power and create necessary 0-255 values
				r = int( ( (color[0] * 255) * brightness ) * (k/fadeSteps) )
				g = int( ( (color[1] * 255) * brightness ) * (k/fadeSteps) )
				b = int( ( (color[2] * 255) * brightness ) * (k/fadeSteps) )

				#iterate through all leds associated with this geography
				for j in range( len( geo["leds"] )):
					#set the pixel for all LEDs per geography to the generated color
					if rasPi == True:
						pixels[geo["leds"][j]] = (r,g,b)
					else:
						pass

			#after all LEDs have been updated, push the data to the actual LED strip
			if rasPi == True:
				pixels.show()

			#wait
			time.sleep(fadeSeconds / fadeSteps)

def pulseData(dataProperty, colormap, pulseCount, fadeSeconds, holdSeconds, fadeSteps=20):
	#show feedback	
	print("Pulsing " + dataProperty + " " + str(pulseCount) + " times in " + str(colormap) + ".")

	#set color map function -- useful for animating LEDs
	if isinstance(colormap, str):
		cmap = plt.cm.get_cmap(colormap)
	else:
		cmap = colormap

	#loop through pulses
	for m in range(pulseCount) :
		fadeData(dataProperty,colormap,fadeSeconds,holdSeconds,True,True,fadeSteps)

def blendData(dataPropertyStart, dataPropertyEnd, colormapStart, colormapEnd, blendSeconds, blendSteps=20):
	#show feedback	
	print("Blending " + dataPropertyStart + " in " + str(colormapStart) + " into " + dataPropertyEnd + " in " + str(colormapEnd) + " over " + str(blendSeconds) + " seconds.")

	#set color map function -- useful for animating LEDs
	if isinstance(colormapStart, str):
		cmapStart = plt.cm.get_cmap(colormapStart)
	else:
		cmapStart = colormapStart
	
	#set color map function -- useful for animating LEDs
	if isinstance(colormapEnd, str):
		cmapEnd = plt.cm.get_cmap(colormapEnd)
	else:
		cmapEnd = colormapEnd

	for k in range(blendSteps):
		#iterate through geographic entities one at a time
		#i is the index, geo is the dictionary of all the data for each geographic entity
		for i, geo in allData.iterrows():
			# for debugging or running on your Mac...
			if rasPi == False and printDebug == True:
				print("For " + str(geo["tractCode"]) + ", lighting up LEDs: " + str(geo["leds"]))

			#determine visualization color for current tract
			#cmaps take a number between 0 and 1 as input, and return a tuple of 3 values between 0 and 1 as a color representation
			colorStart = cmapStart( geo[dataPropertyStart] )
			colorEnd = cmapEnd( geo[dataPropertyEnd] )

			#separate generated color into rgb channels, and manipulate to save power and create necessary 0-255 values
			rStart = int( (colorStart[0] * 255) * brightness )
			gStart = int( (colorStart[1] * 255) * brightness )
			bStart = int( (colorStart[2] * 255) * brightness )

			rEnd = int( (colorEnd[0] * 255) * brightness )
			gEnd = int( (colorEnd[1] * 255) * brightness )
			bEnd = int( (colorEnd[2] * 255) * brightness )

			r = int(rStart + ((rEnd - rStart) * (k/blendSteps)))
			g = int(gStart + ((gEnd - gStart) * (k/blendSteps)))
			b = int(bStart + ((bEnd - bStart) * (k/blendSteps)))

			#iterate through all leds associated with this geography
			for j in range( len( geo["leds"] )):
				#set the pixel for all LEDs per geography to the generated color
				if rasPi == True:
					pixels[geo["leds"][j]] = (r,g,b)
				else:
					pass

		#after all LEDs have been updated, push the data to the actual LED strip
		if rasPi == True:
			pixels.show()

		#wait
		time.sleep(blendSeconds / blendSteps)

def blendTo(dataPropertyStart, colormap, colorEnd, blendSeconds, blendSteps=20):
	#show feedback	
	print("Blending " + dataPropertyStart + " in " + str(colormap) + " into " + str(colorEnd) + " over " + str(blendSeconds) + " seconds.")

	#set color map function -- useful for animating LEDs
	if isinstance(colormap, str):
		cmap = plt.cm.get_cmap(colormap)
	else:
		cmap = colormap

	for k in range(blendSteps):
		#iterate through geographic entities one at a time
		#i is the index, geo is the dictionary of all the data for each geographic entity
		for i, geo in allData.iterrows():
			# for debugging or running on your Mac...
			if rasPi == False and printDebug == True:
				print("For " + str(geo["tractCode"]) + ", lighting up LEDs: " + str(geo["leds"]))

			#determine visualization color for current tract
			#cmaps take a number between 0 and 1 as input, and return a tuple of 3 values between 0 and 1 as a color representation
			colorStart = cmap( geo[dataPropertyStart] )

			#separate generated color into rgb channels, and manipulate to save power and create necessary 0-255 values
			rStart = int( (colorStart[0] * 255) * brightness )
			gStart = int( (colorStart[1] * 255) * brightness )
			bStart = int( (colorStart[2] * 255) * brightness )

			rEnd = colorEnd[0]
			gEnd = colorEnd[1]
			bEnd = colorEnd[2]

			r = int(rStart + ((rEnd - rStart) * (k/blendSteps)))
			g = int(gStart + ((gEnd - gStart) * (k/blendSteps)))
			b = int(bStart + ((bEnd - bStart) * (k/blendSteps)))

			#iterate through all leds associated with this geography
			for j in range( len( geo["leds"] )):
				#set the pixel for all LEDs per geography to the generated color
				if rasPi == True:
					pixels[geo["leds"][j]] = (r,g,b)
				else:
					pass

		#after all LEDs have been updated, push the data to the actual LED strip
		if rasPi == True:
			pixels.show()


		#wait
		time.sleep(blendSeconds / blendSteps)

def blendFrom(colorStart, dataPropertyEnd, colormap, blendSeconds, blendSteps=20):
	#show feedback	
	print("Blending " + str(colorStart) + " into " + dataPropertyStart + " in " + str(colormap) + " over " + str(blendSeconds) + " seconds.")

	#set color map function -- useful for animating LEDs
	if isinstance(colormap, str):
		cmap = plt.cm.get_cmap(colormap)
	else:
		cmap = colormap

	for k in range(blendSteps):
		#iterate through geographic entities one at a time
		#i is the index, geo is the dictionary of all the data for each geographic entity
		for i, geo in allData.iterrows():
			# for debugging or running on your Mac...
			if rasPi == False and printDebug == True:
				print("For " + str(geo["tractCode"]) + ", lighting up LEDs: " + str(geo["leds"]))

			#determine visualization color for current tract
			#cmaps take a number between 0 and 1 as input, and return a tuple of 3 values between 0 and 1 as a color representation
			colorEnd = cmap( geo[dataPropertyEnd] )

			#separate generated color into rgb channels, and manipulate to save power and create necessary 0-255 values
			rEnd = int( (colorEnd[0] * 255) * brightness )
			gEnd = int( (colorEnd[1] * 255) * brightness )
			bEnd = int( (colorEnd[2] * 255) * brightness )

			rStart = colorStart[0]
			gStart = colorStart[1]
			bStart = colorStart[2]

			r = int(rStart + ((rEnd - rStart) * (k/blendSteps)))
			g = int(gStart + ((gEnd - gStart) * (k/blendSteps)))
			b = int(bStart + ((bEnd - bStart) * (k/blendSteps)))

			#iterate through all leds associated with this geography
			for j in range( len( geo["leds"] )):
				#set the pixel for all LEDs per geography to the generated color
				if rasPi == True:
					pixels[geo["leds"][j]] = (r,g,b)
				else:
					pass

		#after all LEDs have been updated, push the data to the actual LED strip
		if rasPi == True:
			pixels.show()

		#wait
		time.sleep(blendSeconds / blendSteps)

def marchData(dataProperty, colormap, fadeSeconds, holdSeconds, fadeSteps=20):
	#show feedback	
	print("Marching through " + dataProperty + " in " + str(colormap) + " over " + str(holdSeconds) + " seconds.")

	#set color map function -- useful for animating LEDs
	if isinstance(colormap, str):
		cmap = plt.cm.get_cmap(colormap)
	else:
		cmap = colormap

	#sort data by desired property
	allData.sort_values(dataProperty)
	
	#iterate over entities one at a time
	for i, geo in allData.iterrows():

		#determine visualization color for current entity
		#cmaps take a number between 0 and 1 as input, and return a tuple of 3 values between 0 and 1 as a color representation
		color = cmap( geo[dataProperty] )
		
		#increase brightness
		for k in range(fadeSteps):

			#separate generated color into rgb channels, and manipulate to save power and create necessary 0-255 values
			r = int( ( (color[0] * 255) * brightness ) * (k/fadeSteps) )
			g = int( ( (color[1] * 255) * brightness ) * (k/fadeSteps) )
			b = int( ( (color[2] * 255) * brightness ) * (k/fadeSteps) )

			#iterate through all leds associated with this geography
			for j in range( len( geo["leds"] )):
				#set the pixel for all LEDs per geography to the generated color
				if rasPi == True:
					pixels[geo["leds"][j]] = (r,g,b)
				else:
					pass

			#after all LEDs have been updated, push the data to the actual LED strip
			if rasPi == True:
				pixels.show()

			time.sleep(fadeSeconds / fadeSteps)

		#wait
		time.sleep(holdSeconds)

		#decrease brightness
		for k in range(fadeSteps, -1, -1):

			#separate generated color into rgb channels, and manipulate to save power and create necessary 0-255 values
			r = int( ( (color[0] * 255) * brightness ) * (k/fadeSteps) )
			g = int( ( (color[1] * 255) * brightness ) * (k/fadeSteps) )
			b = int( ( (color[2] * 255) * brightness ) * (k/fadeSteps) )

			#iterate through all leds associated with this geography
			for j in range( len( geo["leds"] )):
				#set the pixel for all LEDs per geography to the generated color
				if rasPi == True:
					pixels[geo["leds"][j]] = (r,g,b)
				else:
					pass

			#after all LEDs have been updated, push the data to the actual LED strip
			if rasPi == True:
				pixels.show()

			time.sleep(fadeSeconds / fadeSteps)



################################################################################################################
### Make Your Own Colormaps! ###
################################################################################################################
#this site is helpful for color percentage referencing
#https://www.december.com/html/spec/colorper.html

tealToOrangeToPurple = clr.LinearSegmentedColormap.from_list("", [(.22,.72,.80),(.80,.47,.13),(.80,0,.95)])
lemonToLime = clr.LinearSegmentedColormap.from_list("", [(.84,.77,.22),(.2,.8,.2)])

#you can also make color maps that are the same color throughout, to light up all LEDs the same color
allMandarin = clr.LinearSegmentedColormap.from_list("", [(.89, .47, .20),(.89, .47, .20)])



################################################################################################################
### Run Program! ###
################################################################################################################

#loop forever
while True:

	#show all region young person ambulatory disablity percentage as a rank for 5 seconds...
	showData("both535rank", "viridis", 5)

	#send all leds to cyan over 2 seconds
	blendTo("both535rank", "viridis", (0,.9,.9), 2)

	#send all leds to black over 2 seconds
	#can do the same thing with fadeData() by setting fadeUp to False and fadeDown to True
	blendTo("both535rank", "viridis", (0,0,0), 2)

	#fade in young female ambulatory disablity percentage for 2 seconds, hold final values for 1 seconds, fade back down over 2 seconds
	fadeData("female1834ambDisPercentage", "plasma", 2, 1)
	
	#fade in young male ambulatory disability percentage for 1 seconds, hold final values for 2 seconds, fade back down over 1 second, and repeat 3 times
	pulseData("male1834ambDisPercentage", "cool", 3, 1, 2)

	#fade in young male ambulatory disability percentage for 2 seconds, hold final values for 3 seconds, and stop
	#note that, if you made your own colormap, it is not in quotes
	fadeData("male517ambDisPercentage", tealToOrangeToPurple, 2, 3, True, False)

	#blend data from young male ambulatory disability percentage to young female ambulatory disability percentage over 3 seconds
	blendData("male517ambDisPercentage", "female517ambDisPercentage", "inferno", "viridis", 3)
	
	#fade out young female ambulatory disability percentage for 2 seconds, having held the value in place for 3 seconds
	fadeData("female517ambDisPercentage", "viridis", 2, 3, False, True)

	#fade in young female ambulatory disability bins for 1.5 seconds, hold final values for 4 seconds, and stop
	fadeData("female517totalBins", "PuOr", 1.5, 4, True, False)

	#blend to both gender ambulatory disablity rank over 3 seconds
	blendData("female517totalBins", "both535rank", "PuOr", lemonToLime, 3)
	
	#fade out both gender ambulatory disability rank for 2 seconds, having held the value in place for 3 seconds
	fadeData("both535rank", "viridis", 2, 3, False, True)

	#march through all the tracts in order of young female ambulatory disability counts, fading in and out each tract for 1 second, and holding it for 2 seconds
	marchData("female1834ambDisRank", allMandarin, 1, .5)
```
