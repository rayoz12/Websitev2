---
title: Submarine Project
banner_img: ./all_subs.jpg
excerpt: A DIY adventure of using a Pi, Python and Pipes over 5 weeks to deliver a working submarine.
categories: 
    - [IoT]
    - [Python]
tags: 
    - IoT
    - Raspberry Pi
    - Python
    - Sensors
    - Submarine
---

# Remote Operated underwater Vehicle using the building blocks of IoT

**A DIY adventure of using a Pi, Python and Pipes over 5 weeks to deliver a working ROV**


What you see above is the hard work of many groups and right in the middle is our ROV.

During the summer me and a couple of friends (a group of 5) had decided to take up an experimental summer subject where we would be given a project and be expected to complete it in any means possible. The problem posed to us was to build a complete ROV using a set of COTS IoT tools. The tools we were given to make this happen were:

*   2 Raspberry Pis
*   Python
*   Sensors such as:
    *   Camera
    *   Motion / Gyro
    *   Temperature
    *   Underwater compatible Ultrasonic sensors
*   3 Motors

### Why Python in the IoT Domain.

* * *

I choose Python and IOT because it seems like an interesting concept to work with. Another aspect of my studio that caught my attention was the use of python in for embedded systems. Python is not usually targetted to embedded systems as it would be too slow for real time contraints so I wanted to expereince how we would overcome this challenge. The prospect of applying Python to IoT devices otherwise is interesting as you get all the benefits of a high level language while still in an environment constrained by limited resources.

### Requirements and Technical Challenges Associated

* * *

![a picture of the requirements](./requirements.png) A list of requirements as set by the client

  

A requirement's list was given to us to guide us in creating the ROV (certainly one of the better clients). However due to budget and time constraints we could only meet some of these requirements. We decided to focus on:

1.  Live HD Video
2.  Acquiring and Storing Data (in this case pictures and sensors) in the cloud and locally
3.  Highly maneuverable thruster configuration
4.  Easy to use, portable, cross-platform and user interface

In addition we decided to add our own requirements:

5.  Independent Control UI and ROVs so that one Control UI can control multiple ROVs
6.  Integration with a game controller for ease of use

  

#### Technical Challanges

There were certain technical challenges we have to overcome to reach the requirements.

1.  Real time contrained environment with tools that aren't designed for it
2.  Cross Platform & portable
3.  Constructing a ROV body that was waterproof to protect the electronics and one that could move through water.
4.  An Architecture that supported the use of multiple ROVs

We also had the technical challenge of WiFi being unusable underwater but comprimsed on a wired solution.

### Architecture

* * *

![Architecture](./Arch.png)

The control unit essentially acted as a gateway for operators and other applications to access the ROVs. It acted as a router by automatically discovering ROVs connecting to it and displaying it on the Control UI. It also provides a user friendly view for the user to control 1 or many ROVs as well as view the camera and sensor live feed. This was extremely flexible and robot becuase there was nothing that needed to be configure on the ROVs so it could be easily connected above water and deployed.

### Cross Platform & Portable

* * *

As part of the requirements we had to design fwith cross platform in mind, as such we had to use interfaces that were extremely interoperable with other systems. We chose to implement a REST API and a WebSocket Server which offered the same API. The websocket connection however offered faster connection and reliable speeds. The connection interface was written straight into the motor and sensor driver interface for performance reasons. We used the Flask and Websocket library to implement this. The control unit connected to one or many ROVs and sent its commands to the flask server sitting on the ROV. The control unit hosted it's own Flask server for the user or another application to interact with.

  

#### An early revision of the UI

![](./Control_UI.jpg)

### Real time environment

* * *

In IoT use cases a real time environment will be a mandatory constraint. The tool used depends on how much of an effect the environment has on the application. From microncontroller on bare metal for control systems to high level application such as fridges there are varying applications.

In our case performance matters, the feedback loop between the ROV and operator needs to be as small as possible for responsive controls. The client specified Python on the Pi, two tools which aren't targetted to real time constrints. This meant our application had to be as performant as possible. There were several targeted improvements we made:

*   Live streaming solution used for MPEG-DASH (H.264) to enable hardware encoding of video for a responseive camera feed.
*   Seperate channels for each sensor on the ROV so that one sensor read wouldn't stop the others
*   Websockets for 2 way communication
*   Game controller for fine control of motors and movement

### Chassis

* * *

The chassis was arguable the most challenging component of the ROV. With our limited knowledge as software engineers we had to ensure that it would be completely waterproof and prevent any water from getting to the electronics. The advice given to us was to use silicon to proof it as much as we could and we did exactly that. On some shots of the ROV its obvious how much we placed on it. The propelloers attached to the motors were also custom 3d designed and printed for our solution.

In the end however it worked, you can see us testing it out in the video below.

### Continuing On

* * *

Following this project in my own time I set about creating an autonomous Remote control car, reusing much of the Python GPIO interfaces and user interfaces.

### Gallery
**GPIO Connectors from the Pi to the internal motor controllers**
![](./pi_connectors.jpg) 
**Construction of the ROV Chassis**
![](./sub_construction.jpg)  
**Testing the ROV underwater**
![](./underwater_test.png)
**Members of the group**
![](./group_photo.jpg)
