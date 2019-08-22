##### Week 01 Contents
- Presentation: [Syllabus Review, Course Objectives Discussion](readme.md)
- Code: [Python History and Philosophy](python-philosophy.md)
- Code: [Terminal Basics](terminal.md)
- Code: [Python Introduction and Data Manipulation](python.md)
- Components: [Raspberry Pi, Breadboard, Jumper Cables, LED, Resistor, Pushbutton](circuits.md)
- Homework: [Do some research, complete some Python examples, and setup your Raspberry Pi](homework.md)

-----

### Components

#### Raspberry Pi 3b+

![Raspberry Pi 3B+](https://cdn-shop.adafruit.com/970x728/3775-11.jpg)

The [Raspberry Pi](https://en.wikipedia.org/wiki/Raspberry_Pi) is a single-board computers (a "system-on-a-chip" or SoC) developed by scientists at the University of Cambridge and produced in the UK for teaching computer science and electronic basics in educational settings. The RPi, as it is ofted called, is very popular in the electronics hobbyist community, having sold millions of units across several different versions and application spaces including robotics, scientific computation, home automation, game production, and connected object prototyping.

Unlike the related but often-confused [Arduino](https://en.wikipedia.org/wiki/Arduino) specification that exists to serve as an embedded microcontroller without user input, the Raspberry Pi is a standalone computer with usb, mouse, keyboard, sound, wifi, bluetooth, and HDMI video connections usable for interacting with its [Linux-based](https://en.wikipedia.org/wiki/Linux) [Raspbian](https://en.wikipedia.org/wiki/Raspbian) operating system. On its micro-SD card hard drive, users can produce music, play games, write code to control electronic devices, browse the internet, playback media, and anything one could do a traditional desktop machine — albeit at a much slower speed. The Raspberry Pi runs at 1.4GHz, with 1GB of usable RAM. For comparison, baseline MacBook Pros in 2019 also run at 1.4GHz (though they have 4 cores compared to the Pi's 1), with 8GB of RAM. Additionally, the RPi has 40 *general purpose input-output pins*, or GPIO, exposed which can be programmatically controlled in different ways, facilitating the integration of the Raspberry Pi with most small electronic components ever produced including sensors, LEDs and speakers, and screens and cameras.

We will be using the Raspberry Pi as the brain for our projects, creating and running software directly on the device for manipulating electronic components and incoming data. Outside of class, there are [dozens of things you can do with a Raspberry Pi](https://www.makeuseof.com/tag/different-uses-raspberry-pi/), and experimentation is encouraged!


#### Jumper Cable

![jumpers](https://cdn.sparkfun.com//assets/parts/1/1/8/1/JumperWire-Male-01-L.jpg)

More properly a *Dupont Cable*, but often just called *jumpers*, we use these to make electrical connections. The Raspberry Pi operates at very low power settings, with little risk of harming a developer. Nevertheless, better to make connections with wires than fingers! We will often use male->male jumper cables, though female->male and female->female connections are available as well.

Wiring can get confusing, do your best to encode your connection logic in your wire colors and use appropriate wire lengths. 

- Red for 5V Power
- Orange for 3V3 Power
- Black, White, Gray, or Brown for Ground
- Yellow or Purple for Generic Signals
- Green and Blue for I2C Communication

There are many alternatives to wires for making connections — conductive fibers and textiles, silver particle suspension ink, solder, human skin... 

#### Breadboard

![Breadboard](https://cdn.sparkfun.com//assets/parts/9/2/8/7/12615-01.jpg)

The breadboard looks like a boring piece of plastic with some holes in it. But, inside, there are some bits of metal that allows us to prototype connections much more easily, without needing to solder. In landscape orientation...

- Each of the *rows* of holes near the red and blue stripes — on both top and bottom of the breadboard — are connected all the way across. We call these the *rails* of the breadboard, and they are often used to manage power.

- Each of the *columns* inside the *rails* are connected vertically in sets of usually 5. This connection is broken across the middle of the bread board. Individual components often occupy several columns to get power and data in and out. 

![breadboard connections](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTXNJxVaLVXgt4cUjh_Ur4_K5yGOTmLNBGzCKl4EDfxraC-hDyf)

#### LED (Light Emitting Diode)

![LEDs](https://cdn.sparkfun.com//assets/parts/1/2/6/9/7/14563-Green_LEDs_with_built_in_resistor__25_pack_-01.jpg)

LEDs convert electricity into light! They come in many different packages, colors, sizes, beam angles, and brightness levels. Each LED has an anode (long leg, current in) and a cathode (short leg, power out). Though LEDs are often rightfully praised for their low power requirements, high brightness LEDs and LED matrices/strips often require their own dedicated power supplies. Learn more about how LEDs work [here](https://learn.sparkfun.com/tutorials/light-emitting-diodes-leds).

#### Resistors

![pull down resistor](https://cdn.sparkfun.com//assets/parts/8/3/1/08374-02-L.jpg)

Resistors, small pieces of variably conductive ceramic material. are used for two common purposes.

- [Pull-down and pull-up resistors](https://www.electronics-tutorials.ws/logic/pull-up-resistor.html) are used in combination with buttons and other sensors to ensure that current flows and that the state of the pin is always known. In the case of the more common *pull-down resistor*, the goal is to resist all current flowing through the component and leave the component at a clear 0 voltage. In the case of a *pull-up resistor*, the weak resistor ensures the circuit is always receiving current. Without these resistors, component and pin electrical values would fluctuate, or *float*, between high and low and be unknown at any given time. 

- Resistors are also used to limit the amount of electrical power flowing through parts of a circuit, and are termed [current-limiting](https://www.build-electronic-circuits.com/current-limiting-resistor/) when used in this way. They are commonly paired with LEDs, which have very specific current requirements.

Get to know the resistor color code! The amount of electricity resisted by these little bits of ceramic is encoded in their colored stripes. We will talk about how to determine the most correct color code for any application using [Ohm's Law](https://en.wikipedia.org/wiki/Ohm%27s_law) in future weeks.

![resistor color code](http://nearbus.net/mediawiki/images/7/7d/Resistor_color_codes.jpg)

#### Pushbutton

![Pushbutton](https://cdn.sparkfun.com//assets/parts/9/0/00097-03-L.jpg)

Buttons are simple components in complex packagings. They are effectively broken wires, that are fully reconnected when the button is pressed down. This allows a user to control the flow of electrons. Buttons often have multiple exposed pins, allowing several connections to be made on a single press. Our simple pushbuttons have 4 legs total — permanently connected in sets of 2. On depression, all 4 legs get connected together.

![pushbutton](http://razzpisampler.oreilly.com/images/rpck_1102.png)

Buttons come in [many different form factors and sizes](https://www.sparkfun.com/search/results?term=button) for different applications, with slightly different behaviors and defaults, but they all are wired up the same way.

----- 

### Circuits

#### LED Control

Control a set of LEDs by making connections with buttons.

![led controller](led-controller-bb.png)
