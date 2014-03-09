-------------------------------------------------------------
Eyeduino is project for the "Hack the Arduino Robot" contest.
-------------------------------------------------------------

The target of the project is to capture video signal by Arduino and demonstrate usage of it by some exercises.

There is implementation electric schema in the schema_full.png file.

--------------------------------------------------------------------
Following code was developed (successfull part)  during the project:

interrupts1: Used for search of ponts to connect with external interrupt possibility on the Arduino Robot Control Board. Counts and prints logic level change interrupts. Allows to find contacts on the Control Board to which to connect for triggering the interupt.

video10_counters_robot: Simply counts and prints frame interrupts and line/row interrupts to check hardware. Values expected:
- 25 frames per second;
- 25*64 or more lines (after prescaler) per second, depends on camera analog resolution (in lines). 

video11_line_enable_robot: Checks enable feature of the counter/prescaler. After pressing button counter is disabled, stopping interrupts.

video16_comparator_robot: Successfull trial to store value from comparator through digital port due to hardware pre-scaler line interrupt is invoked only for every tenth line of image.
Here I used volatile byte pixel_XX variables to avoid MCU time loss for array index increment. *portInputRegister(6) was used instead of digitalRead() because of much less time needed for execution (see implementation of digitalRead()).
Fine time tuning was made by asm("nop");

video_27_movement_robot_PD: [dotted line folloing exercise] Motors are driven by motorsWrite() function. Successfull trial to implement PD algorithm for dotted line following. Communication to MotorBoard was implemented by functions, copied from RobotControl library and EasyTransfer2 library, renamed to EasyTransfer3. Image capturing part remains the same. calcSpeed() function implements PD regulator.

video_29_cockroach_hunting_servo: [cockroach hunting exercise] Find mass and mass center of black pixels (we consider image to be an image of "cockroach". If mass is less then MASS_IGNORE then we do not move. If mass is between MASS_IGNORE and MASS_AVOID then we attack spot. If mass is greater then MASS_AVOID, then Robot tries to avoid the spot. If spot mass center is near target area of image, weapon (servo) rotates to "kick cockroach".

Step-by-step description of the project is on the Eyeduino Facebook page https://www.facebook.com/eyeduino

------------------------------------------------------------
                   Lessons learned and ideas.
------------------------------------------------------------
1. We used ports, that are regularly used for I2C comunications, so some Arduino Robot features became unavailable. Maybe more comfortable implementation could be done using additional  MCU, say Arduino Nano or Arduino Pro Mini. We could comunicate with Robot Control Board and even Robot Motor Board using I2C protocol. Dotted line following in this case could be implemented as one more mode of Motor Board.
2. Due to short terms of the project we did not unleash the real power of image processing: filters, edge finding, objects recognition etc (of course, in limits of 2K5 memory).
3. Plugin for Arduino IDE (like Serial Monitor) for graphical representation of images, pumped through serial communication. I'll try to do this later.

Best regards
Eduard Petrenko.
