---
layout: post
title: "Basic quadcopter build"
date: 2015-08-05 06:19:21 -0700
comments: true
categories: 
---

A long term desire of mine has been to build a multicopter. Three years ago I tried flying a model powered glider and it went like a stone in a pool. In hindsight a glider is very good at catching any breeze so was a poor choice for a beginner as it was pushed far away before I could get a grip on controlling it. a few trees and almost a window later it was put in a closet then given to Goodwill. A multicopter has a much shorter flight time, but far lower wind load and can be held stationary with little effort even without fancy additions like GPS.
<!-- more -->

As the previous foray had not gone far I did not want to overinvest in the project so chose to do a small quadcopter with a standard frame and popular components. While I still spent far too long selecting components the core philosophy was, if lots of other people are using it there will be lots of resources to help configure and tune the part and total system. This worked wonderfully.

Picking the frame, an EMAX 250, narrowed the selection of other components considerably. The maximum propeller size was 6 inches, but that assumes very good wiring and harnessing of the battery, flight controller, and radio so I went with 5 inch ones. This also provides slightly more crash resiliance as the frame protects a higher proportion of the blades without getting too close while spinning. The motor and ESC controller selections stuck to to the popular and simple route, EMAX 1806 2280KV motors with EMAX 20A ESCs. These ESCs are oversized, both in current capacity and physical dimensions, for the frame but will be useful for future larger frames and were minimally more expensive than the 12A ones. The EMAX ESCs run a slightly modified fork of the SimonK firmware. My flight controller will soon have the functionality to alter ESC parameters

The flight controller is a Naze32 Acro. It runs the CleanFlight firmware, which is a fork of the BaseFlight firmware which is a port of the MultiWii firmware! The open nature of both the hardware and variety of firmwares gives plentiful options to modify and add functionality as desired. The CleanFlight configurator provides a powerful interface for configuring the control loops and flight behavior. The receiver control inputs can have a mapping applied to give logarithmic responses to linear input changes for instance, useful for having both fine control for manouvers and coarse control for avoiding obstacles or stunts. The PID loop algorithm and parameters can be adjusted in realtime even via a serial link. I did not buy the bluetooth bridge for that though.

The transceiver I chose is the Taranis X9D Plus with an X4R receiver. The Taranis runs OpenTX firmware which also has a companion GUI for configuring profiles, both are open source and under active development. For now the configuration is simple, but does have one handy feature. I found that having 100% responsiveness was difficult to handle so Switch A now applies a 50%, 66% or 100% multiplier to all control outputs depending on its state.
