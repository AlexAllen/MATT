# Overview

MATT stands for "Multifunction Altimeter with Telemetry and Tracking", and is a DIY flight computer project that aims to tick several boxes in the [United Kingdom Rocketry Association's (UKRA) Model Achievement Program (MAP)](https://ukra.org.uk/certification/model-achievement-programme/). I have already done the DIY altimeter task, using an Arduino and breakout boards on stripboard, as recorded in [my rocket altimeter repo](https://github.com/AlexAllen/rocket_altimeter), and this project intends to produce something that can tick all of the remaining MAP tasks that require a custom altimeter:

* DIY flight computer
	* Details are light on the exact requirements, but assume a typical dual deploy flight computer
* Satellite
	* A cansat which records some physical data and transmits it back to the ground at 1Hz.
* Land under thrust
	* Need to be able to control servos for thrust vectoring

I also want to use this project as a vehicle for learning about PCB design, and to improve my electronics skills more generally. I intend to use one of the Pimoroni Pico boards as a starting point, and then to create my own PCB to provide the rest of the functionality I require. 

While the resulting flight computer will be overkill for all of these applications, as I don't expect space or weight contraints to be an issue for any of the flights (within reasonable bounds), having them all use the same computer means that every flight can help collect data and test functionality for each of the other applications. If I subsequently find I want a more compact, focussed flight computer for a specific application, then that can be its own project, building on the experience gained here.

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
	* Required to meet Satillite task definition
1. Should transmit on a frequency and in a format that can be picked up by an eggtimer USB ground station
	* Allows reuse of existing equiment
1. Must have enough PWM outputs to control a thrust vectoring mount and deploy legs
	* Required to meet the Land under thrust task task definition
1. Must have sensors that indicate attitude
	* Required to complete the Land under thrust task 
1. Should have a GPS module, and be able to transmit as a GPS tracking radio
	* Makes the result more useful, given an onboard radio is required already
---------------------

# Potential chips/ boards to investigate

## Pimoroni Pico LiPo 2 XL W

Is this worth going for over the standard Pico Plus 2 W with a LiPo shim? It comes with extra pins, but is also longer as a result.

## Hope RF HM-TRP-915 RF module

This is the one used by eggtimer, so should be compatible with the ground station. It is also switchable between 868 MHz and 430 MHz, adds flexibility later.

Pico wifi in the middle

## BMP 390

A likely looking pressure sensor.

## LSM6DSV80X 

A two channel accelerometer, with gyroscope. 16g and 80g should cover most foreseeable use cases

## Quectel L80-R

I have some of these, somewhere. Which is a big point in their favour!

## VN5E160S

These are what the eggtimer quark uses to control deployment charges. 

