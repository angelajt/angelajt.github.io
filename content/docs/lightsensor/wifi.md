---
title: '3. Getting measurements through WiFi'
date: 2021-06-16
draft: false
toc: true
weight: 3
---

## a. Set up a Raspberry Pi with Home Assistant

Follow [the Home Assistant installation instructions](https://www.home-assistant.io/installation/raspberrypi/). 

## b. Install Mosquitto broker

Follow [the Mosquitto installation instructions](https://github.com/home-assistant/addons/blob/master/mosquitto/DOCS.md).

## c. Upload the code

Replace the contents of `code.py` with the following code:

```
import time
import ssl
import json
import alarm
import socketpool
import wifi
import adafruit_minimqtt.adafruit_minimqtt as MQTT
import board
import adafruit_bh1750

PUBLISH_DELAY = 5
MQTT_TOPIC = "state/light-sensor"
USE_DEEP_SLEEP = False

i2c = board.I2C()
sensor = adafruit_bh1750.BH1750(i2c)

# Add a secrets.py to your filesystem that has a dictionary called secrets with "ssid" and
# "password" keys with your WiFi credentials. DO NOT share that file or commit it into Git or other
# source control.
# pylint: disable=no-name-in-module,wrong-import-order
try:
    from secrets import secrets
except ImportError:
    print("WiFi secrets are kept in secrets.py, please add them there!")
    raise


wifi.radio.connect(secrets["ssid"], secrets["password"])
print("Connected to %s!" % secrets["ssid"])

# Create a socket pool
pool = socketpool.SocketPool(wifi.radio)

# Set up a MiniMQTT Client
mqtt_client = MQTT.MQTT(
    broker=secrets["mqtt_broker"],
    port=secrets["mqtt_port"],
    username=secrets["mqtt_username"],
    password=secrets["mqtt_password"],
    socket_pool=pool,
    ssl_context=ssl.create_default_context(),
)

print("Attempting to connect to %s" % mqtt_client.broker)
mqtt_client.connect()

while True:
    fc = sensor.lux * 0.09290304
    print("%.2f footcandles" % fc)
    output = {
        "footcandles": fc,
    }
    mqtt_client.publish(MQTT_TOPIC, json.dumps(output))

    if USE_DEEP_SLEEP:
        mqtt_client.disconnect()
        pause = alarm.time.TimeAlarm(monotonic_time=time.monotonic() + PUBLISH_DELAY)
        alarm.exit_and_deep_sleep_until_alarms(pause)
    else:
        last_update = time.monotonic()
        while time.monotonic() < last_update + PUBLISH_DELAY:
            mqtt_client.loop()
```