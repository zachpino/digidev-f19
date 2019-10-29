##### Week 10 Contents
- Presentation: [Algorithmic Graphics and Form](readme.md)
- Code: [Coding an Image](image.md)
- Code: [MatPlotLib 3D](matplotlib3d.md)
- Code: [Turtle Graphics](turtle.md)
- Code: [Heightfields](heightfield.md)

-----

### Heightfields

We can combine everything that came before this week into a [heightfield/heightmap](https://en.wikipedia.org/wiki/Heightmap), an image-based method of compressing 3D information. In a heightfield image, grayscale values are used to represent elevation. Black is often sea level, and white is mountain peaks. This technique is used in every video game and Pixar movie ever made, and makes storage of large 3D terrains possible. For example, this small heightfield represents terrain data for Mount Kilimanjaro, sourced from [infrared mapping satellites](https://en.wikipedia.org/wiki/Shuttle_Radar_Topography_Mission) participating in global [Digital Elevation Modeling](https://en.wikipedia.org/wiki/Digital_elevation_model) projects. 

This [awesome website](http://terrain.party) makes it easy to grab topographic heightfields.

![kilimanjaro](kili_crop.jpg)

![kilimanjaro](kili_field.png)

```python
from PIL import Image
#for 3d projection and interaction
from mpl_toolkits.mplot3d import Axes3D
#for plotting
import matplotlib.pyplot as plt
#for faster processing and power
import numpy as np

import random

#make a figure
fig = plt.figure()
#set up an subplot in 3D 
ax = ax = fig.add_subplot(111, projection='3d')

#open an image
img = Image.open("kili_crop.jpg")

#get rgb pixel data out of image
pixels = list(img.getdata())

#extract native image size
width, height = img.size

# simple perceptual luminance algorithm from here...
# https://www.scantips.com/lumin.html
def computeBrightness(rgb):
	return ( (rgb[0] * .3) + (rgb[1] * .59) + (rgb[2] * .11) )

x = []
y = []
z = []


#make a figure
fig = plt.figure()
#set up an subplot in 3D 
ax = ax = fig.add_subplot(111, projection='3d')

x = []
y = []
z = []

#construct data
for i in range(height):
	for j in range(width):
		#create a regular grid of points
		x.append(j)
		y.append(i)


for i in range(len(pixels)):
	z.append(computeBrightness(pixels[i]))

# Plot the delaunay triangulation surface
surf = ax.plot_trisurf(x, y, z, cmap='inferno', antialiased=True)

#show the plot!
plt.show()
```