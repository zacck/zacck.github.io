---
layout: post
title: "ATTINY 85 LED BLINKER"
---
# ATTINY85

Class is getting tedious, we are now learning about capacitors, inductors, impendance and so much more and as you can imagine most of class now is theoretical math that we experiment with on the breadboard. I dont have high precision tools(multimeter) so sometimes I can only do the math and speculate which is not harmful at all. I am not short of tools or projects to distract me.

So I have been implementing embedded systems in elixir and rust for a while now but they are usually or a large ARM microprocessor such as the one on the PI or the one on the STM32 or microbit usually a cortex M4 or M3.

Sometimes when I am working on these devices it is hard to debug as I can't 'see' the other side of the line and sometimes you don't know if an issue arises from the rust ecosystem or from the board or maybe even the sensor you are trying to read or write to.

In light of these i decided to dabble in a smaller MCU specifically the attiny85 from the AVR family its an 8bit mcu that has i2c, spi and usi all of which i intend to start using soon on super simple projects just to get more used to lower level debugging and analysis. Also as a backup mcu to test and use sensors when I can't get stuff going in rust.

To that effect I drew and implemented a attiny85 circuit that blinks an LED

The c code for this can be found [here](https://github.com/zacck/attiny85-hello-blink)



I drew a circuit of the processor and the LED
![Drawing of the Circuit](/assets/attiny84_circ.jpeg)

I then built the circuit and flashed the code to it
![Circuit on a Bread Board](/assets/IMG_9176.MOV)



This is the way! See you soon

Happy Today!
