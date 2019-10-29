##### Week 09 Contents
- Presentation: [Updating, Installing, Soldering and Related Skills](readme.md)
- Components: [RGB LED Strips (Neopixels)](circuits.md)
- Homework: [Practice Soldering](homework.md)

-----

### Updating and Installing Some Stuff

### Enable i2c, VNC, and SPI

Let's make sure some common access protocols are enabled. On the Raspberry Pi, via ssh, type...

```
sudo raspi-config
```

Under `interfacing options` enable `I2C`, `VNC`, and `SPI`. `SSH` should remain enabled. Then, select `Finish` and reboot your Raspberry Pi.

-----

### Update and Install Support for Neopixel LED Strips

Let's update our Raspberry Pi to the most recent version of its Raspbian OS.

One line at a time, in your Raspberry Pi's terminal...

These two lines check the Raspberry Pi foundation and other software vendor for `updates`, and then `upgrades` the OS and other software packages.

```
sudo apt-get update
sudo apt-get upgrade
```

Now, following the [instructions in this CoreElectronics tutorial](https://core-electronics.com.au/tutorials/ws2812-addressable-leds-raspberry-pi-quickstart-guide.html)...

Download, install, and build the software binary for handling LED strips

```
curl -L http://coreelec.io/33 | bash
```

Navigate into the downloaded folder

```
cd rpi_ws281x/python/examples/
```

And finally run the demo program! Ensure your Neopixels are wired to 5V, GND, and GPIO 18.

```
sudo python strandtest.py -c
```

You can take a look at this file and edit the LED behaviors...

```
nano strandtest.py
```

-----

### Soldering and Heat Shrink Tubing

![how not to solder](https://pics.me.me/how-not-to-hold-a-soldering-iron-well-this-looks-16549216.png)

Completing a succesful solder joint requires a variety of tough hand-eye skills to master, and it is only through dedicated and repeated practice that it will get any easier. Everyone solders differently, so watching different experts and tutorials may reveal a method that works better for you.

- [Adafruit Soldering Guide](https://learn.adafruit.com/adafruit-guide-excellent-soldering/making-a-good-solder-joint)
- [Sparkfun Soldering Guide](https://learn.sparkfun.com/tutorials/how-to-solder-through-hole-soldering)
- [NASA Splicing Guide](https://www.youtube.com/watch?v=O-ymw7d_nYo) 
- [How to Use Heat Shrink Tubing](https://www.youtube.com/watch?v=Y8wjv6lj5KU)


