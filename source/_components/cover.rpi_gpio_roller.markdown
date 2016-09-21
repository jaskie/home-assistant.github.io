---
layout: page
title: "Raspberry Pi Roller"
description: "Instructions how to setup roller-shutter tubular motor within Home Assistant using the Raspberry Pi and relay module."
date: 2016-08-24 14:28
sidebar: true
comments: false
sharing: true
footer: true
logo: raspberry-pi.png
ha_category: Cover
ha_release: 0.23
---

The `rpi_gpio_roller` cover platform allows you to use a Raspberry Pi to control your roller-shutter bi-directional AC motor such as Somfy. These motors are most popular in alluminium roller-shutters. They have two separated power wires: one for each rolling direction. There isn't allowed to power both direction simultaneously - it can damage motor permanently. These motors also don't requies a sensor - they have internal adjustable limit switch, separately for each direction; therefore they may remain powered some longer than required to fully open or close.

It uses two pins on the Raspberry Pi.

- The `relay_pin_up` will power motor to go up,
- the `relay_pin_down` will power motor to go down.

These pins should output to relay module (such as popular Arduino optoisolated relay modules), because Raspberry Pi itself cannot deliver enough current to drive relay directly with GPIO pins.
Because roller motor cannot be powered simultaneously in both directions, it's reqired to ensure they won't. Simplest method to archieve this is to power one (i.e. controlling motor's up direction) relay AC input with NC output of the second (i.e. down one).

To enable Raspberry Pi Roller in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
cover:
  platform: rpi_gpio_roller
  invert_logic: false
  relay_time: 30
  covers:
    - relay_pin_up: 26
      relay_pin_down: 19
      name: 'Left roller'
    - relay_pin_up: 6
      relay_pin_down: 13
      name: 'Right roller'
```

Configuration variables:

- **covers** array (*Required*): List of your doors.
  - **name** (*Optional*): Name to use in the Frontend.
  - **relay_pin_up** (*Required*): The pin of your Raspberry Pi where the relay to move up is connected.
  - **relay_pin_down** (*Required*): The pin of your Raspberry Pi where the relay to move down is connected.
  - **relay_time** (*Optional*): The time that the relays will be on for in seconds. Default is 30 seconds.
  - **invert_logic** (*Optional*): If relays are activated with low state. Default is `False`
