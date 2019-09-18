##### Week 04 Contents
- Presentation: [Electrical Signalling, Inclusive Carshare+Design Principles](readme.md)
- Components: [Multi LED Circuit](circuits.md)
- Homework Review: [Divvy API Access Code](homework-answers.md)
- Code: [Python GPIO Control](python-gpio.md)
- Homework: [Transit Availability Visualizer](homework.md)

-----

### Python GPIO

![python gpio pins](https://www.jameco.com/Jameco/workshop/circuitnotes/raspberry_pi_circuit_note_fig2.jpg)

We can at last bring together our Python learnings and our small electronics experiments with the useful `GPIO` and `time` Python modules. `GPIO` allows us to turn and off all of the GPIO pins on the Raspberry PI, and `time` lets us pause our program for configurable amounts of time.

##### Digital Control

If we wanted to blink and LED on and off, we could run the following code. Look closely at the comments for more information. Note the `while True :` loop which continues running this code until you press `control + c` on your keyboards.

```python
# import modules
import RPi.GPIO as GPIO
import time  

#store which pin is attached to our LED
ledPin = 21

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
        GPIO.output(ledPin, GPIO.HIGH) 
        #do nothing for one second
        time.sleep(1)             

        #turn off the LED by cutting voltage to its pin  
        GPIO.output(ledPin, GPIO.LOW) 
        #do nothing for one second
        time.sleep(1)                  

# If keyboard Interrupt (CTRL-C) is pressed
except KeyboardInterrupt:
    print("quitting...")
    # safely reset all pins
    GPIO.cleanup()  
```
