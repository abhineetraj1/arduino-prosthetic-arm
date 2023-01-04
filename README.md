# Prosthetic arm
![alt text](https://github.com/abhineetraj1/arduino-prosthetic-arm/blob/main/jk.jpg?raw=true)
Body powered prosthetic hand mechanisms are actuated by human body movement through wires or cables and their goal is to substitute the human hand more esthetically than functionally.

## Electronics
*	3D filament (I used black and white PLA but ABS would work too)
*	At least 8 SG90 servos (I say at least because we will hack them and it possible to destroy them in the process, rendering them useless)
*	Fishing line
*	Thin elastic cord
*	4 springs
*	Heat shrink tube
*	Spare wire
*	Braided nylon sleeve (this is optional, can be hard to find in small quantities, you can probably find an alternative or just use zip ties)
*	Arduino Uno
*	Battery (I used a 9v but a rechargeable lipo would be ideal

## 3D Projection
Fusion are [here](https://willdonaldson.autodesk360.com/g/shares/SH9285eQTcf875d3c539357569a818d905d1)

## Wiring :-
*	A0 - Middle finger potentiometer 
*	6  - Thumb momentary switch
*	7  - Index finger contraction servo
*	8  - Thumb contraction servo (current design does not have a position reading, need to use momentary switch on pin 6)
*	9  - Middle contraction servo (needs pot at A0 to get position reading)
*	10 - Thumb rotation servo 
*	11 - Up/Down wrist rotation servo
*	12 - Left/Right wrist rotation servo
*	13 - Ring and Pinky contraction servo
*	(all wired in parallel to 5V and GND)

## Code Note:-
*	In the code I only have 3 basic hand movements: wave, fist and key hold. Feel free to customize it and add your own.
*	Because of how the potentiometers are orientated in the knuckle the servo has a range of motion of [90, 180] degrees. For example 90 degrees corresponds to the finger being at rest, fully extended, and at 180 degrees the finger is fully contracted. Values outside the range are invalid, and would correspond to bending the finger backwards.
*	The middle finger potentiometer reads values in an analog range of up to 1023, I used the map() function in line 134to convert the range into the same [90, 180] range as the other fingers. The values you map from could be different for different potentiometers, so in setup(), there are several lines dedicated to the calibration of the potentiometer, if you don't wish to recalibrate the pot each time you run the code, you can simply record the calibration values and set the variables MidExtd and MidCont equal to them.
*	As mentioned two steps ago in "Forearm and wiring" the servo controlling the thumb contraction has no position feedback and must be manually controlled with a momentary switch, see the moveThumb() function for more info.
*	Finally as mentioned in the step about how to make a continuous rotation servo, it is important that modifying the servo make sure you rotate the potentiometer to the middle position (ie not to either of the extremes of 0 nor 180 degrees). This is relevant for the thumb contraction and the middle finger contraction as we will write() to either of the extremes: 0 or 180, depending on which way we want to rotate the servo.
*	In the function moveThumb() you my need to change the range of rotAngle from [60, 140] to something different depending on what position the servo was rotated to when attaching the thumb, just play around with the range until you find a suitable one for you.

## Functionality

### Fingers
*	Use stronger elastic, the thin strands I used have a tendency to break
*	Wider hole for the elastic to be fed thru
*	Not all the black finger pieces fit snuggly on the white ones, this prevents smooth rotation of the fingers and    consequently sometimes the tension in the elastic isn’t enough to pull them back to rest and they get stuck.
*	Potentiometers in the knuckle are ineffective as the top segment of the finger (where the fingernail would be)   tends to bend first, followed by the second, meaning no reading is collected until the segment closest to the palm   starts rotating. Thus if something blocks the segment closest to the palm, you have no knowledge of the position   of the top two segments, they will begin rotating why the potentiometer reads the finger is at rest, fully extended.   To overcome this you could design a way to have IMU's in the different segments of the finger, or potentiometers in   each of the points of rotation (both of which I don't see as being practical, given the space restraints and the   number of wires required to fit) or a motor encoder to track how many revolutions the motor makes.
*	Ideally I would actuate the pinky and ring fingers separately, but not of vital importance since you rarely need   to move one without moving the other one.
*	Tying knot in end of elastic is almost impossible while keeping it taught, had to settle for super glue instead. 
    #### Thumb
        *	Axle it spins on is too loose, fortunately this would be a pretty simple fix 
        *	When the finger is contracted, the finger begins to rotate as well as being contracted because the axle mount on   the thumb rotation servo is too loose, the simple fix would be using a bit of epoxy or superglue.
        *	The thumb contraction has no postition feedback, this is perhaps the most serious issue with this design as it can  only be controlled by sight or else it will over contract, breaking either the servo or the thumb. Solutions to  this could involve; some sort of mount for a potnetiometer in the thumb to read the position, use of a reference IMU or perhaps a redesign of how the servos controlling the hand such as using a motor encoder to track rotations.
        *	Some of the issues of the fingers mentioned above in 1.1, translate also into the thumb
    #### Servos 
        ##### Continuous Rotation servos (Index and Thumb Contraction)
            *	Biggest issue is lose of rotation issue as discussed above, better to use small, geared motors with a motor  
                encoder.
            *	Occassionally the fishing line can slip off the pulleys, especially if the fishing line is not under constant  
                tension 
            *	Because the fishing line can wrap around the pulley in both directions, if you are trying to relax the finger  
                by unwinding the pulley and the finger gets stuck (as mentioned above in section 1.1 due to coarse joints) the  
                pulley can keep rotating to the point the fishing line starts wrapping around the pulley in the opposite  
                direction, resulting in the finger being contracted without ever fully relaxing (since the pots only measure 
                knuckle rotation and have no info on the top two segments)
        ##### Compressed 2-in-1 continuous rotation geared dc motors
            *	For the most part this design worked extremely well (since it is "technically" the same as a normal servo, see  
                comments in instructable link for explanation) the biggest issue was my, axle substitute is too narrow,  
                resulting in large vibrations in the gears, later damaging the plastic teeth of the gears actuating the  
                pinky/ring finger combination. Had I found an axle of perfect diameter I believe this would be a null issue.
        ##### Thumb rotation servo
            *	All in all aside from the slipping issue mentioned under section 1.2 this operated as expected.
### Palm
*	Thumb contraction servo is free to move, not enough support walls to hold it in place, instead had to use glue, something I would prefer not to use
*	Ideally I would place an additional screw hole near the pinky finger
*	Serveral of the finger contraction pulleys are too close to the wall, resulting in significant friction, but mostly a simple fix 
*	The fishing line holes for fingers are hard to clean without a long drill bit
*	Hard to service/ take apart if any of the servos break/ fishing line gets twisted.
*	Ideally make more space for new sensors and additional wiring inside palm
### Wrist
*	The servos are too weak to actually rotate the wrist (at least for the springs I used), so while it looks pretty, the wrist doesn't actually do anything
*	While the wrist functions well (I tested it with higher torque servos) it probably won’t work well and may break (depending on the spring and servo strength) if the hand is holding anything of significant weight.
*	Of the 4 screw holes used for mounting the servos in the forearm only 1 can be reached with a screwdriver (oversight on my part), instead I had to use glue
*	The servo responsible for left/right motion (ie waving) could easily be torn out of its mount, instead I should have put a wall on the other side of the servo to prevent it from pulling itself out under its own rotation.
### Forearm
*	The forearm should be made wider so larger, high torque servos such as Kuman MG996 can fit inside without the armatures hitting the walls.
*	Redesigned so the hand can rotate around the forearm axis (ie similar to how the human forearm can roll around its own axis) while the base of the forearm near the elbow remains stationary.
*	The black piece of the forearm needs to have its edges trimmed for a smoother fit inside the white pieces
### 3D printing issues and CAD file issues
*	Nothing too major, mostly that cleaning out support structures can be a challenge in some hard to reach areas
*	As with all projects this large, there is a long printing time, thus the tradeoff between layer height and time must be considered

### Aesthetics 
*	The section where the knuckles meet the palm needs to be smoothed out
*	Not a huge issue but the hand is approximately 10% larger than an adult males hand, in later versions I would like to shrink it
*	The knuckles of a normal human hand arch [1] with the middle knuckle being at the peak of the arch, whereas mine is an (almost) linear progression from pinky being the lowest knuckle to the highest knuckle being the index
*	Perhaps some covers to hide the screws in the palm

## Programming languages used:-
<a href="https://www.w3schools.com/cpp/" target="_blank" rel="noreferrer"> <img src="https://raw.githubusercontent.com/devicons/devicon/master/icons/cplusplus/cplusplus-original.svg" alt="cplusplus" width="40" height="40"/> </a> <a href="https://www.arduino.cc/" target="_blank" rel="noreferrer"> <img src="https://cdn.worldvectorlogo.com/logos/arduino-1.svg" alt="arduino" width="40" height="40"/> </a>

## Author
*	[abhineetraj1](http://github.com/abhineetraj1)
