---
title: '1. Setup'
date: 2021-06-15
draft: false
weight: 1
toc: true
---


## a. Wire it all up

Plug one end of the STEMMA connector into the BH1750, and the other end into the FeatherS2. Then plug the FeatherS2 into your computer with the USB-C cable.

![wiring](wiring.png)

## b. Install some libraries

On your computer, you'll see the FeatherS2 board mounted as a USB drive called `CIRCUITPY`. Inside, you'll see a folder called `lib`. Unless you've already installed libraries on your board, the `lib` folder should be empty. What you'll want to do is get the BH1750 library into this folder so that you can talk to your BH1750 through code.

First, install and unzip the [CircuitPython Library Bundle](https://circuitpython.org/libraries). The FeatherS2 comes with CircuitPython 6, so make sure you install Bundle Version 6.x.

![Screenshot of the correct download button](button.png)

Then look inside the bundle folder and find `adafruit_bh1750.mpy` and the `adafruit_bus_device` folder. Copy both over to your FeatherS2's `lib` folder. **If you're planning to make your light sensor work over WiFi, now would be a good time to copy the folder named `adafruit_minimqtt` over to the `lib` folder as well.**

