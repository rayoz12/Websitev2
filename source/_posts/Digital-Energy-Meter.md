---
title: Digital Energy Meter
tags:
  - IoT
  - C
  - C++
banner_img: ./banner.jpg
index_img: ./projects/img/DSO_Signal.jpg
excerpt: >
  I was given the task to create a digital energy meter on the Freescale K70
  microcontroller. which tracked usage, cost and communication.
categories:
  - - Embedded
  - - IoT
  - - C
  - - C++
date: 2019-08-28 17:28:34
---


# Embedded Development

## Digital Energy Meter
I was given the task to create a digital energy meter on the Freescale K70 microcontroller.
This microcontroller was a low power bare metal system with no OS that had a variety of
modern features.  
This energy meter was given an electrical load reading though an analogue digital converter
and was supposed to track energy usage as well as estimating the cost. 

### Microcontroller Features
This project was the culmination of several weeks of work building up various modules of the
microcontroller.
1. FIFO support backed by the microcontroller
2. UART Communication
3. Communication Protocol
    - A Command oriented command protocol using specially built packet.
    - Used the UART as the main communication channel
4. SPI Controller communication (to an external Analogue to Digital Converter)
5. Several Timers
    - Periodic Interrupt Timer (PIT)
    - Low Power Timer (LPT)
    - Flexible Timers (FTM)
6. Flash Memory
7. Integration of a real time operating system
7. Real Time Clock
8. LED's for communication feedback

In addition software fixed point emulation was also written to support precision math operations
and accuracy.

### The Energy Meter
The energy meter put the above feature together to create a useful tool to monitor electricity usage.
Specifically the meter would sample the electricity reading at a set interval on the PIT
convert that to power and add it to a total on the device. Tariffs which track the cost of power 
would be stored on the flash memory and configurable via the communication protocol. 
The tariffs can vary by time (the use case being peak and off peak rates) and so the device
used the RTC to determine correct tariff. This information without somewhere to output would be useless
and so it would output this information over the UART at request with the eventual idea that it would
output over a display. The communication protocol also allowed the reset and clearing of data to
reset it. During the development phase we also used a special self test mode that generated the
electrical reading. 

This project took around 3 weeks to complete.
