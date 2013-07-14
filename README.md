raspberry-cereal
================

A dash of RPi.GPIO and a sprinkle of python-uinput make for something that maps serial inputs to key presses

Setup
=====
The following commands should take you to victory!
1) Firstly, you need to make sure the uinput kernel module is enabled.
$ sudo modprobe uinput
2) Also nice to make that persistent between reboots.
$ sudo sh -c 'echo uinput >> /etc/modules'
3) This a dependency for python-uinput that we need.
$ sudo apt-get install lidudev-dev
4) And finally, we run setup :)
$ sudo python setup.py install

Raison d'etre
=====
As you may have guessed, this is intended for use on a Raspberry Pi.
This program was quickly written up because the retrogame library from Adafruit (see context: http://learn.adafruit.com/retro-gaming-with-raspberry-pi/overview) could only match pins 1:1 with keys. I needed to attach LOTS of buttons (and two joysticks) so a 1:1 mapping wasn't going to cut it; I needed to serialize my input.
So why not fork and extend retrogame? It uses interrupts, which is awesome and saves CPU time, but just wasn't going to work for serialized input (we unfortunately have to use polling here).

Usage Scenario
==============
A Raspberry Pi hooked up to an 74HC165N shift register taking 8 parallel inputs is what you want. Daisy-chaining a bunch of these will work too, but keep in mind that excessive chaining will come at a performance cost.

Usage
=====
First, you should install it (see above). Then configure it.
The config file is located at /etc/raspberry_cereal.cfg. You're going to want to specify the three pin locations of your 74HC165 (you can use any shift register but you're going to need to modify/extend the functionality to fit your shift register) and then specify which keys you want mapped to which parallel input pin of your shift register.
$ sudo vim /etc/raspberry_cereal.cfg # config as necessary
Now you can go ahead and enable it.
$ sudo raspberry_cereal &
$ ^C

If you would like to daisy-chain a bunch of registers together, just add more keys to the config file as necessary.

Dependencies
============
The setup module should take care of all of the dependencies but should you want to run it without installing, make sure the following are installed.
* python-uinput
* RPi.GPIO
Copying over the config file to /etc/raspberry_cereal.cfg would be good too.
