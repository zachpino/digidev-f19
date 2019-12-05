##### Week 15 Contents
- Presentation: [More to Do with RasPi](readme.md)
- Code Update: [Better Sample Code](project-update.md)
- Logistics: [Course Wrap-Up](wrap-up.md)

-----

Due to the merciless march of the semester calendar, there was a bunch of stuff we did not cover this semester. But, perhaps over your winter break, you might decide to further develop your Python and Raspberry Pi skills by pursuing some of these variably useful applications?

-----

##### Train/Bus/Weather/Bike Warning System

![flips](https://cdn.instructables.com/F5A/I5ZR/IRXTK2BA/F5AI5ZRIRXTK2BA.LARGE.jpg?auto=webp&width=1024&height=1024&fit=bounds)

We did this in class, but take it further! 

- [LCD Train Departure Display](https://www.raspberrypi.org/blog/real-time-train-station-arrival-board/)
- [OLED Train Departure Display](https://www.balena.io/blog/)build-a-raspberry-pi-powered-train-station-oled-sign-for-your-desk/
- [FlipDots Olde-Timey Train Ticker](https://www.instructables.com/id/Howto-Flipdot-With-a-Raspi/)

-----

##### Collect Data with Analog and Digital Sensors! 

![sensors](https://img.dxcdn.com/productimages/sku_142834_1.jpg)

Cameras, thermometers, GPS geolocational data, biomedical human and spatial instrumentation, deployed sensor clusters, environmental data gatherers... We can create our own datasets and APIs, and get the data we need to make much more informed design research choices.

`i2c` or *digital* sensors are much easier to get working on Raspberry Pi than `SPI` or *analog* sensors, which work much more easily with Arduinos.

- [Weather Station](https://www.instructables.com/id/Raspberry-Pi-Solar-Weather-Station/)
- [Adafruit Sensors](https://www.adafruit.com/category/35)
- [Sparkfun Sensors](https://www.sparkfun.com/categories/23?sort_by=name&per_page=200)

-----

##### Retro Video Game Console / Arcade Cabinet / Portable Game System

![snes retropie](https://howchoo.com/media/nw/m0/od/retropie-l.jpeg)

Play the *legally backed-up ROMs you made of your own games* :eyes: with a Raspberry Pi multi-emulator that can play NES, SNES, Atari, PS1, arcade, Saturn and Genesis, and all sorts of other retro games on a [USB retro game pad](https://www.amazon.com/Buffalo-Classic-USB-Gamepad-PC/dp/B002B9XB0E/?tag=n2qyzdk5zdm-20)!

- [RetroPie Official Beginners Guide](https://github.com/retropie/retropie-setup/wiki/First-Installation)
- [RetroPie Alternate Guide](https://pimylifeup.com/raspberry-pi-retropie/)
- [Gameboy Kit](https://www.makeuseof.com/tag/raspberry-pi-gameboy-kit/)
- [Mini Arcade Cabinets](https://www.makeuseof.com/tag/7-fantastic-retropie-game-stations-can-build-weekend/)

-----

##### Make Music with Analog Audio or FM Synthesis!

![Looper](https://camo.githubusercontent.com/7a77960b90dd0534bdc847be6cc135e55cba0564/68747470733a2f2f692e7974696d672e636f6d2f76692f5f6e424b3873416c396e772f302e6a7067)

Create a Raspberry Pi MIDI-enabled synthesizer to create your own music, edit songs, and remix audio files.

- [Official References](https://www.raspberrypi.org/blog/pi-synthesisers/)
- [Looper Synth Project](https://github.com/otem/Raspberry-Pi-Looper-synth-drum-thing)
- [Zynthian](https://zynthian.org)
- [FM Synthesizer](https://hackaday.com/2018/06/08/a-fully-open-source-raspberry-pi-synthesizer/)
- [Junk Drum Machine](https://www.raspberrypi.org/blog/junk-drum-machine/)
-----

##### Ditch your Apple TV/ChromeCast/Roku with a RasPi Media Center

![media center](https://dougbelshaw.com/blog/wp-content/uploads/2012/11/xbmc.jpg)

The excellent Kodi and Plex applications allow the easy management and viewing of all kinds of media files including television episodes, movies, music, mirrored games, streaming content like Netflix and Hulu, podcasts, photographs, whatever! Your Raspberry Pi, hooked up to your TV, an infrared remote control, a hard drive, and your router can become a very powerful home theater driver.

- [Kodi RasPi Media Center](https://www.makeuseof.com/tag/kodi-raspberry-pi-media-center/)
- [Plex on RasPi](https://www.makeuseof.com/tag/raspberry-pi-plex-media-server/)
- [RasPlex](https://www.rasplex.com)

-----

##### Raspberry Pi NAS

![NASty](https://images.ctfassets.net/tvfg2m04ppj4/2TAbWtusZcIU3wG0XbaowD/a5708b325232d2a7e8a0779ee00f7368/main-web.jpg?w=800)

One of the more esoteric, but most useful applications of a Raspberry Pi is as the brains behind a [network attached storage device](https://thewirecutter.com/reviews/best-network-attached-storage/#who-this-is-for). A bunch of cheap hard drives attached to a RasPi and a router enables much more intelligent home file and media sharing, powerful redundant data backups, and opens up your files to you anywhere on the planet.

- [RasPi NAS](https://magpi.raspberrypi.org/articles/build-a-raspberry-pi-nas)

-----

##### Replace a Sonos Multi-Room Audio System

![multiroom](https://support.hifiberry.com/hc/en-us/article_attachments/202216621/1.jpg)

RasPi's make excellent music streaming tools, especially when hooked up to decent speakers. A couple of repurposes large, simple speakers or a cheap order from Amazon allows the  Raspberry Pi to spread music around your entire living space with a ton of control and additional features. Maybe motion sensors carefully positioned through the home could intelligently enable and disable speakers?

https://www.instructables.com/id/Raspberry-Pi-Multi-Room-Audio-MobileTabletPC-Contr/
https://www.instructables.com/id/Sonos-like-Wireless-Multiroom-Sound-System/ 

-----

##### Pirate Radio Station / Walkie-Talkies

![ham radio](https://circuitdigest.com/sites/default/files/projectimage_mic/Raspberry-Pi-FM-Radio-Transmitter_0.jpg)

DJ your own music and share your own talkshows on the *airwaves* with just a wire. Gnarly beats, and only a *little bit* illegal. [Ajit Pai of the FCC probably won't prosecute](https://www.wired.com/story/ajit-pai-man-who-killed-net-neutrality/).

- [RasPi Broadcast Station](https://www.makeuseof.com/tag/broadcast-fm-radio-station-raspberry-pi/)

-----

##### Home Automation

![Mozilla Things](https://hacks.mozilla.org/files/2018/02/things.png)

Make use of the awesomely powerful, automatable, and secure [HomeAssitant](https://www.home-assistant.io) project to turn your residence into a remote-controllable robot without the terrible privacy invasion imposed by traditional consumer electronic home assistants. Control your window shades, thermostat/humidistat, lights, power outlets, gas/electric range, water/plumbing system, music and media systems... without giving up all of your home data to Amazon, Samsung, Apple, or Google.

- [Raspberry Pi Home Assistant](https://www.home-assistant.io/docs/installation/raspberry-pi/)
- [Mozilla Things](https://hacks.mozilla.org/2018/02/how-to-build-your-own-private-smart-home-with-a-raspberry-pi-and-mozillas-things-gateway/)

-----

##### Cloud Printer

![clouds](https://cdn-learn.adafruit.com/assets/assets/000/004/272/large1024/raspberry_pi_seashells.jpg?1396810405)

Mimicking the super-cool-but-now-discontinued [Berg Design Little Printer](https://www.theverge.com/2019/5/19/18628287/little-printer-berg-nord-projects-app-open-source-messaging), get a daily customized newspaper with Sudoku, crosswords, transit notifications, news headlines, horoscopes...

- [Cloud Printer](https://learn.adafruit.com/pi-thermal-printer/parts-list)

-----

##### Magic Mirror

![Magic Mirror](https://miro.medium.com/max/4980/1*ZPTjoOjc7an461l1ZaVqYA.jpeg)

Get your daily news update and contextual information from your bathroom mirror.

- [RasPi Smart Mirror Software](https://docs.smart-mirror.io)
- [Instructable](https://www.instructables.com/id/Smart-Mirror-Raspberry-Pi-3/)

-----

##### AI/Machine Learning Intro

![armadillo!](https://miro.medium.com/max/7538/1*p69rZCP1ydnI55lXQ7LyNg.jpeg)

Get into the most interesting contemporary computer vision and machine learning questions through self-contained learning environments designed to run on the Raspberry Pi.

- [Portable TensorFlow Computer Vision on RasPi](https://towardsdatascience.com/portable-computer-vision-tensorflow-2-0-on-a-raspberry-pi-part-1-of-2-84e318798ce9)

-----

##### Rasapberry Pi Bartender / Espresso Machine

![BitBarista](https://images.ctfassets.net/tvfg2m04ppj4/7x1CWVH3YYA8qU8xKerLoJ/f9088561c75a0fb00aa9b209576c4906/BitBarista.jpg?w=800)

- [BitBarista](https://www.raspberrypi.org/blog/bitbarista/)
- [Smart Robot Bartender](https://www.hackster.io/hackershack/smart-bartender-5c430e)
- [iSpresso RasPi Espresso Machine](https://www.instructables.com/id/Internet-of-Things-RasPi-Espresso-Machine-iSPRESSO/)
- [Es(Pi)resso Machine](http://int03.co.uk/blog/project-coffee-espiresso-machine/)

-----

##### Host a Website

Get rid of your expensive Squarespace Account, and have more control of your public web portfolio by designing and hosting your own website. 

- [Website Host](https://www.makeuseof.com/tag/host-website-raspberry-pi/)

-----

##### BitCoin Miner

![mine mine mine!](https://cdn.instructables.com/F5E/PJAT/I8OBEQ6O/F5EPJATI8OBEQ6O.LARGE.jpg?auto=webp&fit=bounds)

Make some money (maybe?) by harvesting bitcoing with one or more Raspeberry Pi's by inefficiently converting electrical power into a sliver of a BitCoin capital and a whole bunch of heat.

- [BitCoin Miner](https://www.instructables.com/id/Bitcoin-Mining-using-Raspberry-Pi/)

-----

##### Scientific Computing Cluster

![raspi cluster, so many wires!](https://zdnet4.cbsistatic.com/hub/i/r/2014/10/05/444fba83-4c5c-11e4-b6a0-d4ae52e95e57/resize/770xauto/2c37afcdc2cd51514488337a48fd9f19/raspberrypisupercomputer-v1.png)

Linked together into a *cluster*, three Rasperry Pi 3B+ can do more operations per second than Apple's most recent MacBook Pro model. What could you do with eight? What about 64? Photorealistic 3D rendering, photostitching, AI/ML model building, video editing and rendering, solving fluid dynamics, model protein folding to develop pharmaceuticals, searching the universe for alien life... Anything that requires a lot of computing horsepower and automation/batch processing are good candidates.

- [OctaPi](https://projects.raspberrypi.org/en/projects/build-an-octapi)
- [Raspberry Pi Cluster 3D Rendering](https://www.element14.com/community/docs/DOC-86443/l/rendering-3d-with-raspberry-pi-and-bitscope-blade)
- [64 Raspberry Pi Supercomputer](http://www.southampton.ac.uk/~sjc/raspberrypi/)
- [Alien Search with RasPi](https://pimylifeup.com/raspberry-pi-boinc/)
- [Lego Block Raspberry Pi Cluster](https://epcced.github.io/wee_archlet/)
- [4 Node Cluster](https://makezine.com/projects/build-a-compact-4-node-raspberry-pi-cluster/)
- [Folding@Home Cancer Research Cluster](https://foldingforum.org/viewtopic.php?f=38&t=31398)

-----

##### Replace your Expensive Laptop with Open Source Hardware and Software

![kano koala](https://o.aolcdn.com/images/dims?quality=85&image_uri=https%3A%2F%2Fs.aolcdn.com%2Fhss%2Fstorage%2Fmidas%2F69737ee0138b3bd710206ecb47082ad1%2F205706489%2Fkano1.jpg&client=amp-blogside-v2&signature=9b4b8f2c725c11c868e95f33721b57594261d2fe)

Get rid of that expensive hunk of unsustainably manufactured, unrepairable aluminum and run open source software on open source hardware. [GNU IMP](https://www.gimp.org) for image editing, [Inkscape](https://inkscape.org) for vectors, [LAIDout](http://www.laidout.org) for layout and printing, [Blender])(https://www.blender.org) for 3D Modeling....

- [5 Ways to Turn RasPi into Laptop](https://www.makeuseof.com/tag/raspberry-pi-laptop/)
- [Kano Laptop Kit](https://www.amazon.com/Kano-Computer-Kit-Touch-Tablet/dp/B07C7XCJQQ/)

-----

###### Monitor your Bee Colony's Health and Honey Production

:honeybee: :honeybee: :honeybee:

- [RasPi Apiary](https://www.instructables.com/id/Raspberry-Pi-Beekeeping-Server/)

-----

##### Keep Your Family Up to Date with a Smart Photo Frame

![photo frame](https://cdn.instructables.com/FLM/NH3P/HFBKA1NY/FLMNH3PHFBKA1NY.LARGE.jpg?auto=webp&frame=1&width=525&height=1024&fit=bounds)

Share your photos in physical space, and stop any potential familial whining about you disappearing for around 3 months at a time for your 2+ years of grad school.

- [Raspberry Pi Media Panel](https://www.instructables.com/id/How-to-Make-a-Raspberry-Pi-Media-Panel-fka-Digita/)
- [RasPi Photo Frame](https://www.makeuseof.com/tag/showerthoughts-earthporn-make-inspiring-raspberry-pi-photo-frame/)

-----

##### Make Games with Scratch

![maybe only funded a bit with Jeff Epstein Money?](http://u.cubeupload.com/christan/ProjectEditor.png)

Design simple interactive platform, action, and puzzle games with MIT's Scratch software, pre-installed on your Raspberry Pi.

- [MIT Scratch Homepage](https://scratch.mit.edu)
- [RasPi Official Scratch Reference](https://www.raspberrypi.org/documentation/usage/scratch/)

-----

##### BitTorrent Box

:eyes:

- [BitTorrent Box](https://www.howtogeek.com/142044/how-to-turn-a-raspberry-pi-into-an-always-on-bittorrent-box/)

-----

##### Time-Lapse, Stop-Motion, Astrophotography

![astro](https://static.bhphotovideo.com/explora/sites/default/files/styles/top_shot/public/ts-how-to-do-basic-backyard-astrophotography-part1-introduction.jpg?itok=cqI8GCGd)

Raspberry Pi + Camera Module = All sorts of newly accessible photographic approaches

- [Astrophotography with Raspberry Pi](https://www.makeuseof.com/tag/meteors-planets-ufos-raspberry-pi-camera/)
- [Time-Lapse](https://www.raspberrypi.org/documentation/usage/camera/raspicam/timelapse.md)
- [Push-Button Stop Motion Setup](https://www.raspberrypi.org/documentation/usage/camera/raspicam/timelapse.md)
- [DSLR Control](https://pimylifeup.com/raspberry-pi-dslr-camera-control/)
