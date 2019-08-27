##### Week 02 Contents
- Presentation: [Setting Up Raspberry Pi Wifi, SSH, and VLC Access, Inclusive Design Conversation](readme.md)
- New Components: [RGB LED](circuits.md)
- Code: [Moving around with Terminal](terminal.md)
- Code: [Python Basics](python-basics.md)
- Code: [Python Lists and Dicts](python-lists.md)
- Code: [Python GPIO Control](python-gpio.md)
- Homework: [Research, Readings, API Access, Programming Practice](homework.md)
	
-----

### Setting Up Raspberry Pi Wifi

On your SD card, please place the following contents in a file `wpa_supplicant.conf` in `/Volumes/boot/` to connect to networks automatically, without the need for an HDMI screen, keyboard, and mouse.

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

### Setting Up Raspberry Pi SSH

*SSH*, an abbreviation for *secure shell*, is a method to control our Raspberry Pi through another computer.

Add a file, with no content, named "ssh" to your SD card, to enable SSH.

You can do this in Terminal.

```
touch /Volumes/boot/ssh
```

-----

### Setting a Static IP Address

We need our Raspberry Pi to always use the same address when it connects to a network, to prevent address collisions between all the class Pis.

Edit the file `/Volumes/boot/cmdline.txt`. 

At the very bottom, add the following line, where ### is your uniquely assigned number.

```
ip=192.168.1.###
```

This will force your Raspberry Pi to use a certain address on wifi networks. 


OPTIONAL

A different, and slightly more future-proof way to do the same thing would be to boot the Raspberry Pi, and in its Terminal....

```
sudo nano /etc/dhcpcd.conf
```

Add the following lines and change ### to your unique number...

```
interface wlan0
static ip_address=192.168.1.###
static routers=192.168.0.1
static domain_name_servers=192.168.0.1
```

-----

### Remote-Controlling Your Raspberry Pi

On a Mac in Terminal, run the following

```
ssh pi@192.168.1.###
```

You will be asked for a password, which is `raspberry` by default.  :strawberry: <- Not a Raspberry, but close.