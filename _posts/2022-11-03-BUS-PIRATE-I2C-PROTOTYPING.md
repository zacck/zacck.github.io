---
layout: post
title: "I2C prototyping with the Bus Pirate"
---

Embedded development is hard, first off its a combination of a few disciplines such as electronics and software development, that's if you are lucky sometimes your work will even include tasks that need mechatronics skills!

Most of the times when you start an embedded project you will have an idea and for the serious ones some circuit drawings and maybe some proof calculations. After this one usually collects the components of the designed circuit then proceeds to test if each of the  components works and how.

Testing new components is difficult most of them will have no way of observing inputs and outputs, lets say for example you had a project that needs to use data storage and you know one uses [EEPROMS](https://en.wikipedia.org/wiki/EEPROM). One may order an EEPROM that uses say the [I2C Protocol](https://en.wikipedia.org/wiki/I%C2%B2C) and herein lies our problems today, You have the chip how do you test it and find out how it works before integrating it into your project?

If has no visible or audio output so there is no way of observing if it works, integrating straight into your circuit and hoping works is an option, however more often than not this is just a source of a headache than a solution.


## Bus Pirate
In comes the [Bus Pirate](http://dangerousprototypes.com/docs/Bus_Pirate) by Ian Lesnet. The Bus Pirate is a hacker multi tool that talks to electronic stuff. We can use this to troubleshoot almost any embedded device over 1-wire, 2-wire, 3- wire, UART, I2C, SPI and LCD protocols over voltages from 0 - 5.5V.

![Bus Pirate Tool](/assets/bus_pirate.png)

Working with the Bus Pirate is simple! you hook up the tool to your computer via USB and hook into the component you are troubleshooting with the appropriate protocol in our case [I2C](https://en.wikipedia.org/wiki/I%C2%B2C). Once the hookups are done you type commands via a terminal  in your computer and those commands are interpreted and sent via the correct protocol to your component.

![Bus Pirate to USB Connection](/assets/bus_pirate_to_usb.png)



## FT24C256A EEPROM
So on one of my embedded projects and engine hour counter, I need a way to component running hours on an engine and possibly trigger alerts for component services. Now hours and a bunch of text shouldn't take much space so I chose to go with the [FT24C256A](https://www.electronicsdatasheets.com/manufacturers/fremont-micro-devices/parts/ft24c256autrt) a 256K EEPROM. To establish that I can indeed use this device which uses the I2C protocol I need to hook it up, write some data to it and read some data from it.This would prove that the device works and it is observable. One can then use this knowledge when writing the application code later so, let's get testing.

### Hookup to Bus Pirate
Ok so let's start by connecting our EEPROM to the Bus Pirate the connections should be as follows.

|Bus Pirate | EEPROM |
|:----------|:-------|
|GND        | GND    |
|VCC        | 3v3    |
|MOSI       | SDA    |
|CLK        | SCL    |

![How it looks](/assets/ee_to_bus_pirate.png)

Once this is done on Mac let's connect to the Bus Pirate using [screen](https://www.gnu.org/software/screen/) by running the following command

`screen /dev/tty.usbserial-A10L1DXU 115200` this yields a connection screen with a `HiZ>` prompt on this prompt we type `m4` to select the I2C mode and then choose a speed such as `400Khz`

![Initial Connection and Options](/assets/init_bus_connection.png)

### Scan for Device Address(es)
Now let's use the command `(1)` to scan the I2C address space.

![I2C Scan](/assets/bus_1.png)

We can see the device exposes two addresses `0xa1` for reading and `0xa0` for writing, this matches which I2C addressing rules where we use the last bit as a read or write flag. Ok connection established

### Write some Data
Now I am going to read data from the beginning of the storage register and then i'll write some different data to see if writes work.

![Read EE](/assets/bus_read_1.png)

So here we read the first 3 registers and we see they have the values, 4, 56 and 2  lets now write some new values to this registers.

![Write EE](/assets/bus_write.png)

Now here we do a couple of things.

1. We reset the current address to the beginning so we can use the registers we just read.
2. We write the values 5,6,7  to the 3 registers that we have.

Additionally you will notice that the the bus pirate is informing us of protocol actions such as stop and start bits and when they are sent. So it seems we can write to this EEPROM, now does it store the data can we read it back?


###  Read it Back

![Read EE Again](/assets/bus_read_2.png)

Same as above first off we reset the address so that we are dealing with the registers we dealt with before. Then after this we read the registers and guess what! The data that we stored is what we are reading.

Now we know this device works, we know how to use it all before writing a single line of code, sometimes the apparent importance of the tools and techniques we need evades us. I wish I knew of the bus pirate 5 years ago!


Happy today!
