---
title: Writing an AndrOBD Prometheus Exporter
tags:
  - IoT
  - Articles
banner_img: ./grafana.png
index_img: ./projects/img/androbd_grafana.png
excerpt: Analysing my Car's OBD data using Prometheus and Grafana. 
categories:
  - - IoT
  - - Articles
  - - AndrOBD
  - - Prometheus
  - - Grafana
---

# Custom AndrOBD Prometheus Exporter

Have you ever had the itch to figure out what exactly is going on in your when your press the pedal?
The numbers that your Car's ECU (Engine Control Unit) crunches to make the engine move to match the speed
your foot wants? I've had this itch for a while including figuring where I lose my fuel on my daily
trips. I also just want to look at my car data for the pure curiosity of it.

## Architecture
A couple of years ago I had the bright idea of being able to graph my car performance and it took me
those few years to find the pieces to fit it all together. In the meanwhile I had also been moving
towards having a flexible internet aware car that's able to download media (Spotify) and tools (GPS, Waze)
over the internet but also something I can hack on to extend. This came into perfect use for this
project. You can however substitute a normal Android phone like normal. At the start of this project
I only had a vague idea about I it was going to work and with many parts manually written by me. 

<img src="./Initial_Architecture.drawio.png" 
  alt="Intial Architecture"
  style="background-color:white !important; padding:20px;">

This required a lot of work for the web application and I mostly put it off because of things going
on, mainly my work with OpenHarvest. However I understood the main problems I had to solve were:
  1. How would I send the data from the OBD App to the database and what format would it be in?
  2. Designing and storing the data in the database
    1. What would I use? Just plain JSON?
  3. Visualising the data using some sort of library and front end framework.

As time went on I remembered these points but also general purpose software that could be swapped
out and had that functionality. Data storage could be handled as metrics in Prometheus because at
the end of the day it's the same abstract concept of data over time. Visualisation was going to
handled by Grafana because it has excellent support for Prometheus and the graphs it produces are
aesthetically pleasing. Here's the final architecture and I'll elaborate more about AndrOBD, MQTT and
the exporter below.

<img src="./Final_Arch.drawio.png" 
  alt="Final Architecture"
  style="background-color:white !important; padding:20px;">

## Interfacing with my Car - CAN & OBD
Everyone should be familiar enough with the concept of a Local Area Network, also known as a LAN,
but not as many people know there's something in everyone's car (at least from the last 30 years) called
a Car Area Network or a CAN. The CAN bus enables communication between the various parts of the car
but getting your hands on that data is a difficult task and there is no standard format. 
[OBD or On-board Diagnostics](https://en.wikipedia.org/wiki/On-board_diagnostics) is a common method
to view car data in a digital format, the type of data you get about your car varies between models
and implementations. I already had a ELM327 Bluetooth OBD2 adaptor I bought off eBay a few years
back so I chose to use.

![ELM327 OBD2 Device](./obd.jpg)

## Connecting using AndrOBD

### Device
Now that I had the hardware to interface with my car I had to connect another device to the adaptor
over Bluetooth. A while back for the flexibility of having a programmable head unit I installed an
Android head unit in my car. It's been great so far and lends itself well to these types of projects.

![My ATOTO A6](./atoto_a6.jpg)

### Choosing AndrOBD
I also needed some software to communicate over Bluetooth to the OBD device. There are many popular
options on the Play store such as:
  - Torque
  - Car Scanner

However none of them allows you to expose the data you collect over the network to another device.
Looking online for an Open Source OBD app I found 

