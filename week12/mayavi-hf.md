##### Week 12 Contents
- Presentation: [Project Plan](readme.md)
- Presentation: [Milling References](milling.md)
- Code: [Example Choropleth Generator Code](choropleth.md)
- Code: [Better MatPlotLib Heightfields](surface-plot.md)
- Code: [Alternative Mayavi Heightfields](mayavi-hf.md)
- Code: [Example Project Code](project.md)
- Homework: Get to it!

-----

[Mayavi](https://docs.enthought.com/mayavi/mayavi/) is an open source module for Python, as well as a standalone application, for viewing and manipulating 3D data and complex scientific datasets. It is very efficient at triangulation compared to MatPlotLib, and provides for a much nicer experience when engaging heavy data.

Mayavi's Python bindings are designed to be a drop-in replacement for MatPlotLib, so code does not need to change much to switch back and forth between the two libraries. Try it out! 

To install Mayavi...

```
pip install mayavi
pip install PyQt5
```

If you get a yellow 'warning' error when running the second line (Zach did...), Mayavi should still work fine.

Here is a heightfield rendered in Mayavi, with an image sourced from the super cool [Migrations in Motion](http://maps.tnc.org/migrations-in-motion/#5/-4.083/-76.465) project.

![mayavi hf](mayavi.md)

-----

```python
#for image reading
from PIL import Image
#for faster processing and power
import numpy as np
#for hardware accelerated 3D
from mayavi import mlab

#open an image
img = Image.open("terrain.jpg")

#get rgb pixel data out of image
pixels = list(img.getdata())

#extract native image size
width, height = img.size

# simple perceptual luminance algorithm from here...
# https://www.scantips.com/lumin.html
def computeBrightness(rgb):
	return ( (rgb[0] * .3) + (rgb[1] * .59) + (rgb[2] * .11) )

# We need to make sure our height data (measured in brightness from 0 to 255) matches our locational data (measured in pixels in the dimensions of the heightfield image).
# So, in order to bring the dimensions into alignment, we need to relate both latitude and elevation to *actual meters* through a scaling factor. 
# Note that longitude is not considered. There will be slight deformation on longitude as a result — worsening closer to the poles.
# It is impossible to flatten the earth without deformation, so though we could take other complex trigonometric steps that are location-dependent to minimize this deformation, it is in most cases a futile battle.
def zScale(latMin, latMax, heightPixels, elevMin, elevMax, brightnessRange=255) :
	#figure out how much latitude is covered in the image
	coverageLat = abs(latMax - latMin)
	#convert latitude to meters
	#we do this to latitude since the distance between longitude lines varies depending on latitude, but latitude lines are equally spaced
	#111,699 meters per line of latitude
	coverageLatMeters = coverageLat * 111699
	#determine how many meters are represented by each pixel in the image
	latResolution = coverageLatMeters / heightPixels 

	#figure out how much elevation is covered in the image
	coverageElevMeters = abs(elevMax - elevMin)
	#determine how many meters are represented by each brightness step in the image
	elevResolution = coverageElevMeters / brightnessRange

	#return scaling factor
	return latResolution / elevResolution

#all numbers found in the text file that comes with terrain.party downloads
zScaleFactor = zScale(-2.885387,-3.244713, height, 1179, 7174, 255)

#make x and y data
#create an array (like a python list but faster) that looks like [0,1,2,3...] for X and Y, up to the image dimensions
#start at 0, go to width/height, in steps of 1
X = np.arange(0,width,1)
Y = np.arange(0,height,1)

#routine to intermingle X and Y into 2-dimensional arrays
#X looks like [[0,1,2,3...],[0,1,2,3...],[0,1,2,3...]...]
#Y looks like [[0,0,0,0...],[1,1,1,1...],[2,2,2,2...]...]
#this could be replaced with a nested set of for loops, like how Z is done below, but this is much faster.
X, Y = np.meshgrid(X, Y)

#create list holder for Z values
Z = []

#loop through y values
for y in range(height):
	#create a list in the Z list per row of pixels
	Z.append([])
	#loop through x values
	for x in range(width):
		#save a brightness value and scale it to equivalent meters
		Z[y].append(computeBrightness(pixels[ (y*width) + x ]) / zScaleFactor)

#convert list to numpy array for speed and compatibility with X and Y above
Z = np.asarray(Z)

#create mesh from xyz multi-dimensional arrays and a colormap. In the mayavi viewer, the colors can be adjusted.
mlab.mesh(X, Y, Z, colormap='YlGnBu')

#save as obj file for 3d modeling purposes. Also can be done interactively with mayavi viewer.
mlab.savefig('heightfield.obj')

#show the surface! 
mlab.show()