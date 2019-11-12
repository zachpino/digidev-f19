##### Week 12 Contents
- Presentation: [Project Plan](readme.md)
- Presentation: [Milling References](milling.md)
- Code: [Example Choropleth Generator Code](choropleth.md)
- Code: [Example Project Code](project.md)
- Homework: Get to it!

-----

We often saw poor performance when creating heightfield surfaces with MatPlotLib's [plot_trisurf method](https://github.com/zachpino/digidev-f19/blob/master/week10/heightfield.md). A slightly different data structure, and a pinch more complicated code, provides for the creation of simpler and more performative geometries.

The major difference between the two is that the `plot_trisurf` method uses an unstructured [delaunay triangulation](https://en.wikipedia.org/wiki/Delaunay_triangulation) and this `plot_surface` method creates a surface composed of [NURBS-style](https://en.wikipedia.org/wiki/B-spline) structured quadrilateral panels. A discussion of the performance implications can be found in this wiki article on [3D mesh composition](https://en.wikipedia.org/wiki/Types_of_mesh#Quadrilateral).

![quad](quadplot.png)

```python
#for image processing
from PIL import Image
#for 3d projection and interaction
from mpl_toolkits.mplot3d import Axes3D
#for plotting
import matplotlib.pyplot as plt
#for faster processing and power
import numpy as np

#make a figure
fig = plt.figure()
#set up an subplot in 3D 
ax = ax = fig.add_subplot(111, projection='3d')
ax.set_aspect('equal')

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

#make a figure
fig = plt.figure()
#set up an subplot in 3D 
ax = ax = fig.add_subplot(111, projection='3d')
ax.set_aspect('equal')

# We need to make sure our height data (measured in brightness from 0 to 255) matches our locational data (measured in pixels in the dimensions of the heightfield image).
# So, in order to bring the dimensions into alignment, we need to relate both latitude and elevation to *actual meters* through a scaling factor. 
# Note that longitude is not considered. There will be slight deformation on longitude as a result — worsening closer to the poles.
# It is impossible to flatten the earth without deformation, so though we could take other complex trigonometric steps that are location-dependent to minimize this deformation, it is in most cases a futile battle.
def zScale(latMin, latMax, heightPixels, elevMin, elevMax, brightnessRange=255) :
	coverageLat = abs(latMax - latMin)
	coverageLatMeters = coverageLat * 111699
	
	latResolution = coverageLatMeters / heightPixels 

	coverageElevMeters = abs(elevMax - elevMin)
	elevResolution = coverageElevMeters / brightnessRange
	
	#return scaling factor
	return latResolution / elevResolution

#make x and y data
#create an array (like a python list but faster) that looks like [0,1,2,3...] for X and Y, up to the image dimensions
#start at 0, go to width/height, in steps of 1
X = np.arange(0,width,1)
Y = np.arange(0,height,1)
X, Y = np.meshgrid(X, Y)

#create holder for Z values
Z = []

#all numbers found in the text file that comes with terrain.party downloads
zScaleFactor = zScale(-2.885387,-3.244713, height, 1179, 7174, 255)

#create a matching two dimensional array for Z
for y in range(height):
	#create an empty row 
	Z.append([])
	for x in range(width):
		#fill the row with column data from the image
		Z[y].append(computeBrightness(pixels[ (y*width) + x ]) / zScaleFactor)

#convert list to numpy array for speed and compatiblity
Z = np.asarray(Z)

# Plot the quad surface.
surf = ax.plot_surface(X, Y, Z, cmap="viridis", linewidth=0, antialiased=True)

#the following lines of code calculate the 3d midpoint and bounds of the surface, and use these to equalize all axes
#find the largest of the axes
max_range = np.array([X.max()-X.min(), Y.max()-Y.min(), Z.max()-Z.min()]).max() / 2.0
#find the midpoints on all the axes
mid_x = (X.max()+X.min()) * 0.5
mid_y = (Y.max()+Y.min()) * 0.5
mid_z = (Z.max()+Z.min()) * 0.5
#set the limits based on the above values, centering the geometry and equalizing the axes
ax.set_xlim(mid_x - max_range, mid_x + max_range)
ax.set_ylim(mid_y - max_range, mid_y + max_range)
ax.set_zlim(mid_z - max_range, mid_z + max_range)

#turn off meaningless axis labels
ax.set_axis_off() 

#show the stupid surface! 
plt.show()
```