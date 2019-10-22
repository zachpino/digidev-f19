##### Week 09 Contents
- Presentation: [Soldering and Related Skills](readme.md)
- Components: [RGB LED Strips (Neopixels)](circuits.md)
- Homework: [Practice Soldering and RGB Strip Visualization](homework.md)

-----

### Components

#### RGB LED Strip (WS2812)

![RGB LED Strip](https://cdn-shop.adafruit.com/970x728/1507-00.jpg)

A series of LEDs, all wired together, that can be addressed in code with simple Python methods. Amazing, bright, cheap, easy. Learn more at the [Adafruit overview](https://learn.adafruit.com/neopixels-on-raspberry-pi/overview).

Be aware that the LED strip has very specific power requirements. For easy math, assume each LED needs 60mA of power at max white brightness -- so your power supply needs to be able to supply `(60 * Number of LEDs) / 1000` amps of power. Very often, we would want to use a separate power supply allocated just for the LED strips if many lights are needed, for example in architectural applications.

![stairs with leds](https://cdn11.bigcommerce.com/s-43185/product_images/uploaded_images/stairsled.jpg)

Never buy the over-priced [Philips Hue light strips](https://www.amazon.com/Philips-Ambiance-LightStrip-Compatible-Assistant/dp/B0167H33DU)! You can instead get inexpensive, [standard WS2812 led strips](https://www.amazon.com/gp/product/B00VQ0D2TY/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1) and a compatible [IoT controller](https://www.amazon.com/Zigbee-Controller-Compatible-Lightify-Control/dp/B07FTD9H7T/ref=sr_1_1_sspa?keywords=hue+compatible+led+controller&qid=1571767266&s=hi&sr=1-1-spons&psc=1&spLa=ZW5jcnlwdGVkUXVhbGlmaWVyPUEzMFo1R0E1NjRJVVRDJmVuY3J5cHRlZElkPUEwMDIyMzg5MVdJUExTVzE5TEZBQiZlbmNyeXB0ZWRBZElkPUEwMjgwODcyMVQxR05TNEJJVlVIUyZ3aWRnZXROYW1lPXNwX2F0ZiZhY3Rpb249Y2xpY2tSZWRpcmVjdCZkb05vdExvZ0NsaWNrPXRydWU=).

----- 

### Circuit

##### Up to 16 LEDs with a 1 Amp Power Supply

(60 * 16) / 1000 = .96 Amps -- doable with just 1 amp of available power, but just barely.

![RGB LED Strip](https://cdn-learn.adafruit.com/assets/assets/000/063/929/medium640/led_strips_raspi_NeoPixel_bb.jpg?1539981142)

-----

### Adafruit's Sample Code for NeoPixels

```python
# Simple test for NeoPixels on Raspberry Pi
import time
import board
import neopixel
 
 
# Choose an open pin connected to the Data In of the NeoPixel strip, i.e. board.D18
# NeoPixels must be connected to D10, D12, D18 or D21 to work.
pixel_pin = board.D18
 
# The number of NeoPixels
num_pixels = 30
 
# The order of the pixel colors - RGB or GRB. Some NeoPixels have red and green reversed!
# For RGBW NeoPixels, simply change the ORDER to RGBW or GRBW.
ORDER = neopixel.GRB
 
pixels = neopixel.NeoPixel(pixel_pin, num_pixels, brightness=0.2, auto_write=False,
                           pixel_order=ORDER)
 
 
def wheel(pos):
    # Input a value 0 to 255 to get a color value.
    # The colours are a transition r - g - b - back to r.
    if pos < 0 or pos > 255:
        r = g = b = 0
    elif pos < 85:
        r = int(pos * 3)
        g = int(255 - pos*3)
        b = 0
    elif pos < 170:
        pos -= 85
        r = int(255 - pos*3)
        g = 0
        b = int(pos*3)
    else:
        pos -= 170
        r = 0
        g = int(pos*3)
        b = int(255 - pos*3)
    return (r, g, b) if ORDER == neopixel.RGB or ORDER == neopixel.GRB else (r, g, b, 0)
 
 
def rainbow_cycle(wait):
    for j in range(255):
        for i in range(num_pixels):
            pixel_index = (i * 256 // num_pixels) + j
            pixels[i] = wheel(pixel_index & 255)
        pixels.show()
        time.sleep(wait)
 
 
while True:
    # Comment this line out if you have RGBW/GRBW NeoPixels
    pixels.fill((255, 0, 0))
    # Uncomment this line if you have RGBW/GRBW NeoPixels
    # pixels.fill((255, 0, 0, 0))
    pixels.show()
    time.sleep(1)
 
    # Comment this line out if you have RGBW/GRBW NeoPixels
    pixels.fill((0, 255, 0))
    # Uncomment this line if you have RGBW/GRBW NeoPixels
    # pixels.fill((0, 255, 0, 0))
    pixels.show()
    time.sleep(1)
 
    # Comment this line out if you have RGBW/GRBW NeoPixels
    pixels.fill((0, 0, 255))
    # Uncomment this line if you have RGBW/GRBW NeoPixels
    # pixels.fill((0, 0, 255, 0))
    pixels.show()
    time.sleep(1)
 
    rainbow_cycle(0.001)    # rainbow cycle with 1ms delay per step
 ```