# Overview

MATT stands for "Multifunction Altimeter with Telemetry and Tracking", and is a DIY flight computer project that aims to tick several boxes in the [United Kingdom Rocketry Association's (UKRA) Model Achievement Program (MAP)](https://ukra.org.uk/certification/model-achievement-programme/). I have already done the DIY altimeter task, using an Arduino and breakout boards on stripboard, as recorded in [my rocket altimeter repo](https://github.com/AlexAllen/rocket_altimeter), and this project intends to produce something that can tick all of the remaining MAP tasks that require a custom altimeter:

* DIY flight computer
	* Details are light on the exact requirements, but I'm assuming a typical dual deploy flight computer
* Satellite
	* A cansat which records some physical data and transmits it back to the ground at 1Hz.
* Land under thrust
	* Need to be able to control servos for thrust vectoring

I also want to use this project as a vehicle for learning about PCB design, and to improve my electronics skills more generally. I intend to use a microcontroller board (maybe a Pico of some sort, or a Teensy) as a starting point, and then to create my own PCB to provide the rest of the functionality I require. I might do this in stages, creating some simpler boards with a subset of the final functionality on the way towards the final board. 

While the resulting flight computer will be overkill for any of these applications, as I don't expect space or weight contraints to be an issue for any of the flights (within reason), having them all use the same computer (or at least a subset of the same computer) means that every flight can help collect data and test functionality for each of the other applications. If I subsequently find I want a more compact, focussed flight computer for a specific application, then that can be its own project, building on the experience gained here. 

---------------------

# Requirements

1. Must have Mach safe two channel (apogee and main) deployment triggered off pressure data
	* Required to meet DIY flight computer task definition
1. Must have buzzer for audible altimeter arming alerts
	* Required for safety when completing DIY flight computer task
1. Must record flight data in a robust way in flight (not to an SD card!), and make the data retrievable afterwards
	* Will make the result more useful, and make it easier to test
1. Should have individual wireless test functionality for both deployment channels
	* Makes the result more flexible, but could be a later update
1. Must have UK legal radio link capable of transmitting recorded data at 1Hz
	* Required to meet Satellite task definition
1. Must record temperature data as well as pressure data, for transmission
	* Required for Satellite task
1. Should transmit on a frequency and in a format that can be picked up by an eggtimer USB ground station
	* Allows reuse of existing equiment
1. Must have enough PWM outputs to control a thrust vectoring mount and deploy legs
	* Required to meet the Land under thrust task task definition
1. Should have enough PWM outputs to control 4 canards instead of thrust vectoring and legs
	* Makes the result more future proof
1. Must have accelerometers and gyroscopes
	* Required to complete the Land under thrust task 
1. Should have magnetometers
	* Makes the result more flexible
1. Should have a GPS module, and be able to transmit as a GPS tracking radio
	* Makes the result more useful, given an onboard radio is required already
	
---------------------

# Potential chips/ boards to investigate

## Microcontroller

### Pimoroni Pico 2 Plus W / Pico LiPo 2 XL W

The Pico 2 Plus W was my starting point before I started looking at the Teensy alternatives. I already have some, and have used them at least a little, so that's a plus in their favour. They also either come with, or have options for coming with, some features I want already built in. This includes wifi, and some lipo functionality. It also has bluetooth, if I want to use that instead of wifi. 

The Pico LiPo 2 XL W has the lipo functionality built in already, and is longer as a result (76mm vs 51mm), but also comes with more pins. They are both 21mm wide.

Useful links:

* [Pimoroni Pico 2 Plus W](https://shop.pimoroni.com/products/pimoroni-pico-plus-2-w?variant=42182811942995)
* [Pimoroni LiPo Shim](https://shop.pimoroni.com/products/pico-lipo-shim?variant=32369543086163)
* [Pimoroni Pico LiPo 2 XL W](https://shop.pimoroni.com/products/pimoroni-pico-lipo-2-xl-w?variant=55447911006587)


### Teensy 4.0 or 4.1

This looks to have a significantly more powerful processor than a pico plus 2, and hardware floating point calculations, so I'll have plenty of grunt for calculations, but have to do more stuff myself (wifi, etc). The Teensy 4.1 is a larger and more fully featured board than the Teensy 4.0, at about 61mm vs 36mm (both are about 18mm wide). The 4.1 has the option to add 2x 8MB QSPI chips directly onto the board, and a built in microSD card reader, so I could look at writing some data storage/ retrival code before the PCB is complete.

Useful links:

* [Pi Hut Teensy 4.1](https://thepihut.com/products/teensy-4-1)
* [Pimoroni Teensy 4.1](https://shop.pimoroni.com/products/teensy-4-1?variant=31757218250835)
* [Teensy 4.1 documentation](https://www.pjrc.com/store/teensy41.html)

Or:

* [Pi Hut Teensy 4.0](https://thepihut.com/products/pjrc-teensy-4-0-usb-development-board)
* [Pimoroni Teensy 4.0](https://shop.pimoroni.com/products/teensy-4-0-development-board?variant=29443577217107)
* [Teensy 4.0 documentation](https://www.pjrc.com/store/teensy40.html)


## RF modules

### ESP8266-12 WiFi Module

If I go for the Teensy over the pico, then I don't get wifi built in. This is the wifi module that eggtimer uses.

### Hope RF HM-TRP-868 RF module

This is the one used by eggtimer, so should be compatible with the ground station. There is also an otherwise identical 433 MHz version, which may be useful later.

## Sensors

### BMP 390 (or 388, or 280, or ...)

A temperature and pressure sensor. I used the BMP 280 on my [rocket altimeter](https://github.com/AlexAllen/rocket_altimeter) with good results, and I have one on a breakout board to play with already. According to the Adafruit overview, the BMP280 has a relative accuracy of 12 Pa, the BMP388 8 Pa, and the BMP390 3 Pa. That translates to about 1m for the BMP280 or about 0.25m for the BMP390. Seems to be recommended on TRF threads about building flight computers, so this is probably the one to go for.

### LSM6DSV80X 

A two channel accelerometer, with gyroscope. 16g and 80g should cover most foreseeable use cases. The LSM6DSO32 or similar seem to be recommended on TRF, but this looks like an updated version that includes an 80g rather than a 32g high range accelerometer.

### LIS3MDL

This magnetometer is discussed on [TRF](https://www.rocketryforum.com/threads/magnetometer-calibration.193930/), where it seems to be a reasonable choice.

### Quectel L80-R

I have some of these, somewhere. Which is a big point in their favour!

### Something from ublox?

People seem to rate these highly, might be worth investigating

## Other

### VN5E160STR-E

These are what the eggtimer quark uses to control deployment charges. According to Cris Erving of Eggtimer Rocketry, ["in one SOIC-8 package you get 10A current limiting, continuity checking, brownout protection (it shuts off the load if the input voltage drops below 4.5V), overtemperature protection, and they will work with up to 40V input."](https://www.rocketryforum.com/threads/flight-computer-schematic-review-request.186506/post-2588670).
	
---------------------

# Other useful things:

### Ulyu (Silicdyne) talks about how to implement an IMU

[This thread on TRF](https://www.rocketryforum.com/threads/did-any-one-found-out-a-way-to-calculate-the-velocity-and-altitude-using-imu-sensors-with-practical-code.191467/post-2722289)

### Cris Erving (Eggtimer) suggests owning this book

[The Circuit Designer's Companion](https://www.amazon.co.uk/Circuit-Designers-Companion-Wilson-Professor/dp/0081017642)
