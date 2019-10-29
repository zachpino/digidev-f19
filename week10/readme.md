##### Week 09 Contents
- Presentation: [Soldering and Related Skills](readme.md)
- Components: [RGB LED Strips (Neopixels)](circuits.md)
- Homework: [Practice Soldering and Get Sensors Working](homework.md)

-----

### Updating and Installing Some Stuff

One line at a time, on the Raspberry Pi, via ssh, confirming as needed with 'y'...

```
sudo apt-get update
sudo apt-get upgrade
sudo pip3 install --upgrade setuptools
sudo apt-get install python3-pip
pip3 install adafruit-blinka
sudo pip3 install rpi_ws281x adafruit-circuitpython-neopixel
```

-----

### Enable i2c and SPI

Let's make sure some common access protocols are enabled. On the Raspberry Pi, via ssh, type...

```
sudo raspi-config
```

Under `interfacing options` enable `I2C` and `SPI`. Then, select `Finish` and reboot your Raspberry Pi.

-----

### Soldering and Heat Shrink Tubing

![how not to solder](https://pics.me.me/how-not-to-hold-a-soldering-iron-well-this-looks-16549216.png)

Completing a succesful solder joint requires a variety of tough hand-eye skills to master, and it is only through dedicated and repeated practice that it will get any easier. Everyone solders differently, so watching different experts and tutorials may reveal a method that works better for you.

- [Adafruit Soldering Guide](https://learn.adafruit.com/adafruit-guide-excellent-soldering/making-a-good-solder-joint)
- [Sparkfun Soldering Guide](https://learn.sparkfun.com/tutorials/how-to-solder-through-hole-soldering)
- [NASA Splicing Guide](https://www.youtube.com/watch?v=O-ymw7d_nYo) 
- [How to Use Heat Shrink Tubing](https://www.youtube.com/watch?v=Y8wjv6lj5KU)


