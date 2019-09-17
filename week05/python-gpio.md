##### Week 05 Contents
- Presentation: [Inclusive Bus Stop Principles and Design, PWM Signalling](readme.md)
- Components: [RGB Color Control Circuit](circuits.md)
- Code: [Python PWM Control](python-gpio.md)
- Homework Review: [Transit Availability Visualizer](homework-answers.md)
- Homework: [Transit Availability Visualizer Add-Ons](homework.md)

-----

### Advanced Python GPIO Control

![python gpio pins](https://www.jameco.com/Jameco/workshop/circuitnotes/raspberry_pi_circuit_note_fig2.jpg)

We can again bring together our Python learnings and our small electronics experiments with the useful `GPIO` and `time` Python modules. `GPIO` allows us to turn and off all of the GPIO pins on the Raspberry PI, and `time` lets us pause our program for configurable amounts of time. We will add to our knowledge of the `GPIO` module by exploring its `PWM` functionality today.

##### PWM Control

![led array!](https://cdn.sparkfun.com//assets/parts/1/2/8/3/2/RGB_Panel_Blinking_Goodness.gif)

Very often, we want more control than the binary on/off covered last week that `GPIO.HIGH` and `GPIO.LOW` allow. Towards this end, to simulate *analog control*, we will send a modulated signal to an LED. This signal will flicker at a frequency that the human eye cannot preceive, resulting in a variable brightness that we can control. We call these signals *pulse-width-modulated*, abbreviated to *PWM*, which are especially useful for color control and animation with RGB LEDs. Check out [this tutorial](https://electronicshobbyists.com/raspberry-pi-pwm-tutorial-control-brightness-of-led-and-servo-motor/) for a review of what we discussed in class and an in-depth discussion of PWM signals.

```python
#necessary modules
import RPi.GPIO as GPIO     
import time

#store pin locations in variables
rPin = 26
gPin = 19
bPin = 13

#use BCM numbering system
GPIO.setmode(GPIO.BCM)          

#set all needed pins as output
GPIO.setup(rPin, GPIO.OUT)   
GPIO.setup(gPin, GPIO.OUT)   
GPIO.setup(bPin, GPIO.OUT)   

#create a PWM object for each pin, with 100 as its pulse frequency in hertz
rPWM = GPIO.PWM(rPin, 100)    
gPWM = GPIO.PWM(gPin, 100)    
bPWM = GPIO.PWM(bPin, 100)    

#startup the PWM signal at 0 duty load
rPWM.start(0)
gPWM.start(0)
bPWM.start(0)

#make sure we can safely exit our program by looking out for errors
try :
    #loop forever
    while True :
        #count up to 100
        for i in range(100) :
            #change PWM pulse
            rPWM.ChangeDutyCycle(i)
            bPWM.ChangeDutyCycle(100 - i)
            #delay a tiny bit
            time.sleep(0.01)
        
        #count down to 0    
        for i in range(100,0,-1) : 
            #change PWM pulse
            rPWM.ChangeDutyCycle(i)
            bPWM.ChangeDutyCycle(100 - i)
            #delay a tiny bit
            time.sleep(0.01)

# If keyboard Interrupt (CTRL-C) is pressed
except KeyboardInterrupt:
    print("quitting...")
    # safely reset all pins
    rPWM.stop()      
    gPWM.stop()      
    bPWM.stop()      
    GPIO.cleanup()
```
