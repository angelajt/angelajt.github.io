---
title: '2. Getting measurements through USB'
date: 2021-06-15
draft: false
toc: true
weight: 2
---

## a. Upload the code

The code to talk to the light sensor is very simple. Edit `code.py` on `CIRCUITPY` so that the file contains just these lines (this code was taken from [Adafruit's BH1750 tutorial](https://learn.adafruit.com/adafruit-bh1750-ambient-light-sensor/python-circuitpython#example-code-3066441-17)):

```python
import time
import board
import adafruit_bh1750

i2c = board.I2C()
sensor = adafruit_bh1750.BH1750(i2c)

while True:
	print("%.2f Lux" % sensor.lux)
	time.sleep(1)
```

If you'd like to measure light in foot-candles instead of lux, use this code (which converts the lux reading to foot-candles):

```python
import time
import board
import adafruit_bh1750

i2c = board.I2C()
sensor = adafruit_bh1750.BH1750(i2c)

while True:
	fc = sensor.lux * 0.09290304
  print("%.2f footcandles" % fc)
	time.sleep(1)
```

## b. Get measurements

To see the output from your sensor, you'll need to connect to your FeatherS2's serial console. This is easy if you're using [Mu Editor](https://learn.adafruit.com/welcome-to-circuitpython/installing-mu-editor) which is recommended for CircuitPython, but in my case I was having trouble installing it so I followed [CiruitPython's advanced serial console](https://learn.adafruit.com/welcome-to-circuitpython/advanced-serial-console-on-mac-and-linux) instead.

![Screen command](command.png)

![Light readings](output.png)
