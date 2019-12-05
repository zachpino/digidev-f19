##### Week 12 Contents
- Presentation: [Project Plan](readme.md)
- Presentation: [Milling References](milling.md)
- Code: [Example Choropleth Generator Code](choropleth.md)
- Code: [Better MatPlotLib Heightfields](surface-plot.md)
- Code: [Alternative Mayavi Heightfields](mayavi-hf.md)
- Code: [Example Project Code](project.md)
- Homework: Get to it!

-----

More to come including [point in polygon](https://automating-gis-processes.github.io/CSC18/lessons/L4/point-in-polygon.html) example.

```python
#necessary modules
import requests
import json
import time
import matplotlib.pyplot as plt
import geopandas as gpd
import pandas as pd
import numpy as np
from shapely.geometry import Point, Polygon
#from neopixel import *

# LED strip configuration:
LED_COUNT      = 16      # Number of LED pixels.
LED_PIN        = 18      # GPIO pin connected to the pixels (18 uses PWM!).
#LED_PIN        = 10      # GPIO pin connected to the pixels (10 uses SPI /dev/spidev0.0).
LED_FREQ_HZ    = 800000  # LED signal frequency in hertz (usually 800khz)
LED_DMA        = 10      # DMA channel to use for generating signal (try 10)
LED_BRIGHTNESS = 255     # Set to 0 for darkest and 255 for brightest
LED_INVERT     = False   # True to invert the signal (when using NPN transistor level shift)
LED_CHANNEL    = 0       # set to '1' for GPIOs 13, 19, 41, 45 or 53

#create neoPixel object
#strip = Adafruit_NeoPixel(LED_COUNT, LED_PIN, LED_FREQ_HZ, LED_DMA, LED_INVERT, LED_BRIGHTNESS, LED_CHANNEL)

#turn on our connection to our strip
#strip.begin()

#key for census API
censusKey = ""

#census variables of interest
dataPoints = "B18105_002E,B18105_004E,B18105_007E,B18105_010E,B18105_013E"

#construct census API
#17 is FIPS for Illinois, 031 is FIPS for Cook County
url = "https://api.census.gov/data/2017/acs/acs5?get=NAME,B01001_001E," + dataPoints + "&for=tract:*&in=state:17&in=county:031&key=" + censusKey

#grab data and parse
censusResponse = requests.get(url).json()

#create empty list as a future data container
dataList = []

#loop through census data -- note that we start at index "1" because the first data entry is a header row
for i in range (1, len(censusResponse)) : 

	#skip row, as there is no population at this census tract 
	if censusResponse[i][1] == "0":
		#skip!
		pass
	#all the rows that show a population
	else:
		#create dictionary of meaningful data pre-computed
		dataList.append(
			{
				"name": censusResponse[i][0],
				"totalPopulation": int(censusResponse[i][1]),
				"totalMale": int(censusResponse[i][2]),
				"5-17": int(censusResponse[i][3]),
				"18-34": int(censusResponse[i][4]),
				"35-64": int(censusResponse[i][5]),
				"65-74": int(censusResponse[i][6]),
				"totalMaleAmbulatoryDisability": int(censusResponse[i][3]) + int(censusResponse[i][4]) + int(censusResponse[i][5]) + int(censusResponse[i][6]),
				"percentageMaleAmbulatoryDisability": ( int(censusResponse[i][3]) + int(censusResponse[i][4]) + int(censusResponse[i][5]) + int(censusResponse[i][6]) ) / int(censusResponse[i][1]),
				"tractCode" : censusResponse[i][9],
				"sqrtPop": (int(censusResponse[i][1]))**.25

			}
		)


#convert data into a pandas data frame, which is like an excel table in code
frame = pd.DataFrame(dataList) 

#load shapefile and reproject to a better map projection
#you can look up EPSG map projection coded here...
#https://spatialreference.org
#2790 is a good choice for north-eastern IL
geography = gpd.read_file('bounds/geo_export_ea609890-7468-44a7-99ef-edc77453045d.shp').to_crs(epsg=2790)

#combine data! 
#left_on is the column header for the *geography*, the data receiving the merged data
#right_on is the column header in the data frame from the *census*, the data being injected in the merge
merged = geography.merge(frame, left_on='tractce10', right_on='tractCode')

#led index associations
leds = [
	{"tractCode": "600900", "leds":[0,7]},
	{"tractCode": "839800", "leds":[8]},
	{"tractCode": "840000", "leds":[16]},
	{"tractCode": "340500", "leds":[17]},
	{"tractCode": "842000", "leds":[32,33]},
	{"tractCode": "839500", "leds":[48]},
	{"tractCode": "600600", "leds":[1]},
	{"tractCode": "840200", "leds":[2,3,4,5,6,11]},
	{"tractCode": "600400", "leds":[9,10]},
	{"tractCode": "840100", "leds":[14,15]},
	{"tractCode": "340300", "leds":[14,15]},
	{"tractCode": "340400", "leds":[18,19]},
	{"tractCode": "350400", "leds":[30,31]},
	{"tractCode": "839200", "leds":[34,35,46,47]},
	{"tractCode": "841100", "leds":[12,13,20,21,22]},
	{"tractCode": "841000", "leds":[28,29,36,37,45]},
	{"tractCode": "330200", "leds":[23,24,25,26,27]},
	{"tractCode": "330100", "leds":[38,39,40,41,42,43,44]}
]

#convert data into a pandas data frame, which is like an excel table in code
leds = pd.DataFrame(leds) 

#join leds into our data collection based on both tables having a 'tractCode' column
merged = merged.merge(leds, left_on='tractCode', right_on='tractCode')

#throw away all census tracts that don't have LED assignments
cropped = merged[merged.tractCode.isin(leds["tractCode"])]

#set color value property for sending to LEDs
cropped["colorValue"] = (cropped["percentageMaleAmbulatoryDisability"] * 255).astype(int)

#set color map function -- useful for animating LEDs
cmap = plt.cm.get_cmap('viridis')

#main led loop
while True:

	#iterate through tracts in a for loop
	for i,row in cropped.iterrows():
		print("Lighting up LEDs: " + str(row["leds"]))
		print("According to value: " + str(row["percentageMaleAmbulatoryDisability"]))
		print("With color: " + str(cmap(row["percentageMaleAmbulatoryDisability"])))
		
		#iterate through led associated with this tract.
		# for j in row[leds]:
			#strip.setPixelColor(row["leds"][j], Color(cmap(row["percentageMaleAmbulatoryDisability"])))
			#strip.show()

	time.sleep(5)


#draw map!
#color maps here: https://matplotlib.org/3.1.0/tutorials/colors/colormaps.html
#legend=True shows the color map bar
cropped.plot(column="sqrtPop", cmap='Greys_r', linewidth=0, edgecolor='#ffffff', legend=True)

#save as vector with ".svg"
#save as raster with ".jpg" or ".png"
#plt.savefig("test.svg")
#plt.savefig("test.jpg")

#plt.title("Choropleth — Share of Total Population, Males with Ambulatory Impairment")
#turn off axes display, doesn't make much sense for a map
#plt.axis('off')

#show the stupid plot!
#plt.show()

```



