##### Week 03 Contents
- Presentation: [Inclusive Design Small Group Chats, Electrical Signalling](readme.md)
- Components: [RGB LED Color Controller](circuits.md)
- Code: [Python Lists and Dictionaries](python-lists.md)
- Code: [Python GPIO Control](python-gpio.md)
- Application: [Train Arrival Warning Alarm](application.md)
- Homework: [Readings, Other APIs, Programming Practice](homework.md)
    
-----

### Python GPIO

![python gpio pins](https://www.jameco.com/Jameco/workshop/circuitnotes/raspberry_pi_circuit_note_fig2.jpg)

We can at last bring together our Python learnings and our small electronics experiments with the useful `GPIO` and `time` Python modules. `GPIO` allows us to turn and off all of the GPIO pins on the Raspberry PI, and `time` lets us pause our program for configurable amounts of time.

##### Digital Control

If we wanted to blink and LED on and off, we could run the following code. Look closely at the comments for more information.

```python
# import modules
import RPi.GPIO as GPIO
import time  

#store which pin is attached to our LED
led = 21

#ignore some default warnings that will always pop up
GPIO.setwarnings(False)

#This next line is super confusing!
#There are two pin numbering map schemas, "BCM" and "BOARD". "BCM" is short for "Broadcom," the fabricator of the Raspberry Pi processor. See the image at the bottom of this page for more info.

#"BCM" is what we will usually choose, and routes the voltage signals to the pin as it is counted on the processor directly.
#This is what leads to the numbers on the cobbler being so weird, with the pin numbering showing no pattern in their placement (13,19,26 are next to each other!)
#Different Raspberry Pi versions have placed the pins in different orders, so we usually want to use BCM numbering.

#"Board" would consider the top-left pin to be pin 1 and below it to be pin 0, with even numbers in the bottom row and odd in the top row. 
#There are very few uses for this schema, as code written this way would only work on one version of the Raspberry Pi.
#Take a look at the pinout diagram above for more info!

GPIO.setmode(GPIO.BCM)     

#We need to tell GPIO pins if they should listen or speak -- here we are telling our LED pin to send voltage out, rather than listen for voltage in.
GPIO.setup(led, GPIO.OUT)   

#make sure we can safely exit our program by looking out for errors
try :
    #Run forever! 
    while True :
        #turn on the LED by sending voltage to its pin
        GPIO.output(led, GPIO.HIGH) 
        #do nothing for one second
        time.sleep(1)             

        #turn off the LED by cutting voltage to its pin  
        GPIO.output(8, GPIO.LOW) 
        #do nothing for one second
        time.sleep(1)                  

# If keyboard Interrupt (CTRL-C) is pressed
except KeyboardInterrupt:
    print("quitting...")
    # safely reset all pins
    GPIO.cleanup()  
```

##### PWM Control

Very often, we want more control than the binary on/off that `GPIO.HIGH` and `GPIO.LOW` allow. For this we want PWM, which is especially useful for color control and animation with RGB LEDs. Check out [this tutorial](https://electronicshobbyists.com/raspberry-pi-pwm-tutorial-control-brightness-of-led-and-servo-motor/) for a review of what we discussed in class and an in-depth discussion of PWM signals.

```python
#necessary modules
import RPi.GPIO as GPIO     
import time

#store pin locations in variables
r_pin = 26
g_pin = 19
b_pin = 13

#use BCM numbering system
GPIO.setmode(GPIO.BCM)          

#set all needed pins as output
GPIO.setup(r_pin, GPIO.OUT)   
GPIO.setup(g_pin, GPIO.OUT)   
GPIO.setup(b_pin, GPIO.OUT)   

#create a PWM object for each pin, with 100 as its max duty load
r_pwm = GPIO.PWM(r_pin, 100)    
g_pwm = GPIO.PWM(g_pin, 100)    
b_pwm = GPIO.PWM(b_pin, 100)    

#startup the PWM signal at 0 duty load
r_pwm.start(0)
g_pwm.start(0)
b_pwm.start(0)

#make sure we can safely exit our program by looking out for errors
try :
    #loop forever
    while True :
        #count up to 100
        for i in range(100) :
            #change PWM pulse
            r_pwm.ChangeDutyCycle(i)
            b_pwm.ChangeDutyCycle(100 - i)
            #delay a tiny bit
            time.sleep(0.01)
        
        #count down to 100    
        for i in range(100,0,-1) : 
            #change PWM pulse
            r_pwm.ChangeDutyCycle(i)
            b_pwm.ChangeDutyCycle(100 - i)
            #delay a tiny bit
            time.sleep(0.01)

# If keyboard Interrupt (CTRL-C) is pressed
except KeyboardInterrupt:
    print("quitting...")
    # safely reset all pins
    r_pwm.stop()      
    g_pwm.stop()      
    b_pwm.stop()      
    GPIO.cleanup()
```
