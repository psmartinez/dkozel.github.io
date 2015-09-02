---
layout: post
title: "Pelco Pan Tilt head"
date: 2015-08-05 06:18:48 -0700
comments: true
categories: 
---

For some time I've been working on a setup for receiving from, and eventually transmitting to, satellites and other moving aerial targets such as high altitude balloons. The initial design was based off of the work done by the [SatNOGS](https://satnogs.org/) team, but ran into reliability issues when 3D printing a revised version of the helical antenna joints. Additionally the SatNOGS design is being iterated at the moment and their rotator design wasn't complete. In searching for alternative elevation azimuth rotator designs a few options came up:

* Build a SatNOGS style rotator from servos
* Join two single axis satellite dish rotators
* Use a manual pan tilt head such as the Quickset Herculese line
* Buy a commercial two axis antenna rotator
<!-- more -->

Each of these was a viable design with examples of working systems easy to find online. However given limited time and money non was a good match for me. Then another option came up, using a pan tilt head for CCTV cameras! These are built for a high duty cycle, heavy loads, remote operation, and long life spans. The trade off is that they are generally expensive, have odd power requirements, and often quite heavy. The control systems also come in a variety of physical styles and protocols. The nicest ones have RS 485 interfaces with well documented protocols.

What I ended up with is a Pelco PT280-24P head with a MPT24DT controller which doubles as a power supply. The head's motors run directly off of 24VAC lines coming from the controller which has a single interface, a joystick. The controller unfortunately runs off of 120VAC so I'm looking into buying a 12VDC to 24VAC boost inverter so I can run it either from an easily sourced wall brick or from batteries/solar while portable. The joystick interface should be simple to integrate with using some analog outputs. However there is no positioning feedback from the head so that's another feature which will need to be added. I'm not sure if using one of the inexpensive accelerometer/gyro/magnetometer boards is overkill so will probably start with simple potentiometer based feedback. The PT280-24P/PP model includes this feedback as well as circuitry to move the camera to specific set points. Documentation online indicates that the internal hardware is missing so a simple breakout modification is not possible. Another option is to add a camera with a computer vision based feedback, but this is almost certainly adding complication for the sake of being fancy.

The head and controller are in the mail now so I'll start work on the system after returning from an upcoming conference.
