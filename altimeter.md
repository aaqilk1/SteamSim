# Altimeter
 
This file will go over all of the reverse engineering for the altimeter module found on the ATC-710. As with the other motor/servo-driven instruments, this one contains
a 12V DC motor/gearbox module and continously rotating 50k-ohm potentiometer. For the altimeter specifically, it seems that the needles are connected to a clock mechanism
which causes the "thousands" hand to move 1 number for every full revolution of the "hundreds" hand. Interestingly enough, it seems that the potentiometer is connected to 
the thousands hand so the readout *should* be consistent for altitudes from 0-10.000ft. There is also a momentary switch that can be heard clicking at 2 and 8 on the dial.

One issue that will probably present itself later is what the potentiometer will read as it gets close to the wrap point. This is probably why the momentary switch is part
of the mechanism. 

Here are some photos of the altimeter gauge removed from the main chassis. There is a barometer readout on the front that can be adjusted with the knob but it doesnt seem to
be connected to any of the potentiometers. I could potentially (hah!) connect a pot/hall effect sensor/rotary encoder to it later to compensate for barometric pressure and 
plug that information into the sim. 

![IMG_4634](https://github.com/user-attachments/assets/8474cd4d-0d94-4eb7-baca-a6de7bb5343f)
This is the front of the altimeter. The adjustment dial on the bottom left adjusts barometric pressure. 

![IMG_4635](https://github.com/user-attachments/assets/5992db2f-ee57-4218-9ca2-5af745ab604c)
This is the rear of the altimeter. The two blue wires at the bottom drive the servo motor, while the black/green/brown wires drive connect to the infinite rotation potentiometer. The yellow and green wires near the gear wheel connect to the momentary switch. 

![IMG_4636](https://github.com/user-attachments/assets/3cbe46d0-3fae-4b47-93d0-d194491b41f5)
Detail showing the momentary switch mechanism better. 

To test the altimeter and figure out the potentiometer reading, I set it up to an arduino with a cheap L293D motor driver shield. 

![IMG_4633](https://github.com/user-attachments/assets/2be3c46f-cb8f-40c4-bb6f-652d23505225)
In this setup, the motor wires are connected to L293D Motor 1 Output. The potentiometer is connected between 5V/GND with wiper (pin 2) 
plugged into A0. There are two momentary push buttons connected to D5/D6 and ground. To prevent frying the arduino's voltage regulator, I've connected external power to the L293D shield and have 12.5V running from the benchtop power supply. 

To try to calibrate the displayed values with potentiometer readings, I made a simple table on Microsoft Word, then printed it and filled in the arduino pot values as I manually drove the needle to the displayed altitude. The chart (and calibration values) are below. These were baked into the arduino code so that I could send a value over the serial port and have the gauge automatically move to that location. 

The code for reverse engineering the Altimeter allows for manual adjustment using the momentary buttons and has Serial to read out the potentiometer values. The snippet listed under <altimeter_code> allows for manual override with the buttons, but also allow
