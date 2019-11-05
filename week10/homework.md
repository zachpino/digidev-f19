##### Week 10 Contents
- Presentation: [Algorithmic Graphics and Form](readme.md)
- Code: [Coding an Image](image.md)
- Code: [MatPlotLib 3D](matplotlib3d.md)
- Code: [Heightfields](heightfield.md)
- Homework: [LED Strips, Image Manipulation, and Heightfields](homework.md)

-----

### Homework for November 5

##### Soldering Practice (1 hour)

- Make some jewelry and fashion to practice soldering! Take some wires and create some bracelets and anklets, necklaces, braided wire art, pins, garment add-ons, anything you like. Bonus points if you solder in some batteries and LEDs (don't forget resistors)! 

-----

##### Hardware Coding (1.5 hours)

Add 3 wires onto your LED strip so you can connect it to your breadboard, then...

Connect your [Raspberry Pi and LED strip](../week09/readme.md) to one of the several APIs we have discussed in class so far (census, bus tracker, train tracker, divvy status, something else?). Create a *dynamic visualization* using your LED strip that physicalizes some data from the selected API(s). Make sure you power your Raspberry Pi **via its wall plug**, not over USB, when using the LED strip.

For example, could the LED strip represent a series of stops on certain bus or train stops near your home, with color used to communicate the nearness of the approaching vehicle?  The overall logic is up to you -- but try to make it useful and legible.

-----

##### Software Coding (1.5 hours)

Three tasks based heavily on the code from this week....

1. First, create a script that converts a color image to a grayscale image. Could you then write a script that does the reverse, and red/green/blue color versions from grayscale images like [Photoshop's Hue/Saturation Colorize Adjustment](https://helpx.adobe.com/photoshop-elements/using/adjusting-color-saturation-hue-vibrance.html#adjust_saturation_and_hue)? Hint for both: consider borrowing the `computeBrightness` function from the [edge detection](image.md) example. 

2. Create a script that removes all *red color* from an image and replaces it with *green*. Could you extend this to be more flexible, partially recreating [Photoshop's Selective Color Adjustment](https://photographypla.net/introduction-selective-color-adjustment/)? Hint: consider using the HSV color space rather than RGB!

3. Create a [heightfield of your own](heightfield.md) based on an image, or on a part of the world downloaded from [terrain.party](http://terrain.party)? 

Bonus: Could you write a script that takes two images of equal pixel dimensions, and combines the two together into a new image with alternating pixels? The first pixel comes from image A, the second pixel comes from image B, the third pixel comes from image A... This is a form of spycraft encryption called [steganography](https://en.wikipedia.org/wiki/Steganography). Could your script be easily editable to allow more images? Or easily exhibit different pixel patterns like AAABAAABAAAB? 

-----

##### Readings (1 hour)

- Bess Williamson's recent [*Accessible America*](https://drive.google.com/drive/folders/1lRB-g2c6-mOYRbo-Usb9As9pjDypJPDH), PDF Pages 145-179.

	This chapter narrates the increasing importance of *Universal Design* as a movement in the contemporary design world, and the many antecedents that led up to it. Williamson is trying to link together, with this chapter, the two contrary threads of designing for inclusion and accesibility that we have identified together — and I'm hopeful that some new ideas and compromises might emerge from this short chapter - especially its last few pages. 

- Visit these webpages so we can talk about them next week informedly.
	- [Chicago in Miniature](https://www.buildyourownchicago.com/modelcity.html)
	- [Building Footprint Dataset](https://data.cityofchicago.org/Buildings/Building-Footprints-current-/hz9b-7nh8) and [its documentation](https://data.cityofchicago.org/api/assets/003C600C-3A66-4605-8E7E-2477AAE95E16)

- (Optional) We did not have a chance to follow-up on the conversation that occured over email regarding disability through obesity, the often false appearance of *choice in disability*, the varying scopes of inclusive thought, user centered design's culpability in the obesity epidemic, and design's potential role as remediator. Some ongoing work and thinking in this topic space — certainly not comprehensive, but perhaps stimulative:
	
	- [The Complicated Problem of Urban Obesity](https://www.citylab.com/equity/2016/04/obesity-is-a-city-problem/476547/)
	- [Can Urban Design Help Solve the Obesity Crisis?](https://www.verywellhealth.com/urban-design-as-solution-for-obesity-epidemic-4072010)
	- [Obesity relationships with community design, physical activity, and time spent in cars](https://www.ajpmonline.org/article/S0749-3797(04)00087-X/fulltext)
	- [Combating Obesity with Design](https://www.architectmagazine.com/practice/combating-obesity-with-design_o) 

