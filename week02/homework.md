##### Week 02 Contents
- Presentation: [Installation, Inclusive Design Conversation](readme.md)
- Code: [Moving around with Terminal](terminal.md)
- Code: [Python Basics](python-basics.md)
- Homework: [Readings, API Access, Programming Practice](homework.md)

-----

### Homework for September 3

##### Programming Practice (2 hours)

Complete exercises 1, 2, 3, 4 and 8 at [Practice Python](https://www.practicepython.org), and come with questions! Answers are on the same page. You will need to do some things we did not cover in class, so give these exercises a try, and we will talk about them next week.

-----

##### Readings (2 hours)

- Read [Kat Holmes' *Mismatch*](https://drive.google.com/drive/folders/1lRB-g2c6-mOYRbo-Usb9As9pjDypJPDH?usp=sharing), pages 84 - 123 and prepare for discussion. Please write down *on paper* three questions that this reading provoked in your mind to discuss with your peers.

- Read this short Guardian article [*What would a truly disabled-accessible city look like?*](https://www.theguardian.com/cities/2018/feb/14/what-disability-accessible-city-look-like). 

- Skim this short excerpt *Transportation Patterns and Problems of People with Disabilities* from [*The Future of Disability in America*](https://www.ncbi.nlm.nih.gov/books/NBK11420/). The whole book is available in the shared readings folder. Again, three questions on paper please!

-----

##### Data Exploration (1 hour)

Request a key, and read the documentation for the [Chicago Train Tracker API PDF](https://www.transitchicago.com/assets/1/6/cta_Train_Tracker_API_Developer_Guide_and_Documentation.pdf). In particular, study the *Arrivals API* as described in the PDF pages 5-10. 

An API, or *Application Programming Interface* is a structured exchange protocol for accessing realtime, or near realtime, data. If we follow the rules and procedures that the developer has set up for us, we can access all kinds of amazing data. This particular API is very simple, and requires just a bit of preparation. With a correctly formatted request, we will be able to get predicted arrival time information on any trains on their way toward a certain CTA station, as well as useful information about the trains themselves like heading, longitude+latitude coordinates, and maintenance state.

We access this API just like we would visit any website address — by manipulating the contents of a URL. We need to start with a *base*, or *stem* API address. The PDF makes clear that the arrivals API address base is...

```
http://lapi.transitchicago.com/api/1.0/ttarrivals.aspx?
```

If you visit that URL in a web browser, you'll find a simple XML error message saying that our request is missing necessary content including a *key* and *mapid*/*stpid*/*stnid*.

Let's work on those! We can easily [request a key](https://www.transitchicago.com/developers/traintrackerapply/), which is how the CTA will keep track of your 50,000 requests/day limit and log how different users access the API services and resources.

Your key will be a 32 character long alphanumeric string. Copy it to continue structuring your API request.

```
http://lapi.transitchicago.com/api/1.0/ttarrivals.aspx?key=################################
```

One more piece is needed, the identifier for the target station. We can specify this in many ways, but the `mapid` is the easiest. Pages 27 - 32 lists all of the stations, with the `mapid` in the rightmost column. We also could use `stpid` instead of `mapid` if we were only interested in trains traveling in **one** direction at each stations. These are listed in the leftmost column.

For `mapid`...

```
http://lapi.transitchicago.com/api/1.0/ttarrivals.aspx?key=################################&mapid=#####
```

Or for `stpid`...

```
http://lapi.transitchicago.com/api/1.0/ttarrivals.aspx?key=################################&stpid=#####
```

Lastly, and this is a matter of opinion, but many developers find the [JSON](https://www.w3schools.com/js/js_json_intro.asp) formatting language easier to read, rather than [XML](https://www.w3schools.com/xml/xml_whatis.asp) (the default formatting style you are seeing that looks much like HTML tags). You can toggle your query to be JSON instead, but there is absolutely no difference in content.

```
http://lapi.transitchicago.com/api/1.0/ttarrivals.aspx?key=################################&mapid=#####&outputType=JSON
```

We will work with this data next week on our Raspberry Pis in Python, to create a train arrival warning system for the studio. Get familiar with it, and come with questions!

-----

##### Raspberry Pi Wifi Setup (15 Minutes)

- Setting Up Raspberry Pi Wifi

Place your Raspberry Pi SD card in the USB reader in your kits, and plug it into your computers. please place the following contents in a file `wpa_supplicant.conf` in `/Volumes/boot/` to connect to networks automatically, without the need for an HDMI screen, keyboard, and mouse.

In Terminal, type or paste these lines one at a time and hit enter after each.

```
cd /Volumes/boot/
touch wpa_supplicant.conf
open -a TextEdit wpa_supplicant.conf
```

Copy and paste everything below into the empty file that opens in TextEdit. You can insert your home wifi name and password into the text in the second `network` block so that your Raspberry Pi can connect at home and school! Save the file with File -> Save.

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

network={
    ssid="pddev"
    psk="raspberry"
    key_mgmt=WPA-PSK
}

network={
    ssid="homewifiname"
    psk="homewifipw"
    key_mgmt=WPA-PSK
}
```

-----

##### Raspberry Pi SSH Setup (5 Minutes)

*SSH*, an abbreviation for *secure shell*, is a method to control our Raspberry Pi through another computer.

Add a file, with no content, named "ssh" to your SD card, to enable SSH.

You can do this in Terminal if the Rasperry Pi SD card is still plugged in via the USB adapter.

```
touch /Volumes/boot/ssh
```

-----

### Remote-Controlling Your Raspberry Pi (20 minutes)

Make sure you are the only one in the classroom doing this step! All Raspberry Pis use the same default network name, so everyone will need to do this step alone to prevent name collisions.

Plug your SD card back into your Raspberry Pi. Plug it into power and turn it on, making sure that its onboard LED glows green. Wait 5 minutes for everything to boot up.

Then, on your Mac in Terminal, run the following.

```
ssh pi@raspberrypi.local
```

You will be asked for a password, which is `raspberry` by default.  :strawberry: <- Not a Raspberry, but close.

After a slight delay, you are now running code on your Raspberry Pi! You know this because Terminal changes your prompt.

```
pi@raspberrypi:~$
```

Now, to prevent future name collisions...

```
sudo nano /etc/hostname
```

This will open a simple file in your Terminal for editing. Replace `raspberrypi` with a unique name (for example "zach-raspberry"), using only numbers, hyphen, and letters.

After typing your unique name, write it down for later reference, and then hold down the control key and press 'x' on your keyboard. The text editing program will ask if you want to save your changes. Press 'y' and hit enter.

Then, type in the following...

```
sudo reboot now
```

In order to log into your Raspberry Pi from now on, we would type in on your laptop...

```
ssh pi@youruniquename.local
```

Test this! In order to safely turn off your Raspberry Pi if you are logged into it...

```
sudo shutdown -r now
```