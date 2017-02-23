---
title: "Final Year Project - Preliminary Report"
excerpt: "This is the preliminary report I submitted for my Final Year Project in DT080B"
---

# Introduction

The purpose of this Preliminary Report is to explain the objectives of the project and provide a timeline of events that need to be achieved. This will be done in the following manner:

- Breaking down the project into a number of tasks
- Describing these tasks
- Researching existing solutions
- Talking about research areas that will be covered
- Creating a plan with times to complete each objective

# Objectives

The objective of this project is to develop a sensor platform that will report soil moisture, temperature and light level back to a central server, which will display these statistics in a graph over a web interface. This will involve determining what single board computer (SBC) to use, along with sensors, communication methods, data storage methods and methods for displaying the stored information. The SBC will also be able to drive a solenoid for the purpose of watering the plant.

The key objectives that must be achieved are as follows:

1. Identification of a suitable SBC and development environment
2. Identification of a suitable web service
3. Select, source and interface with sensors
4. Select, source and interface with pump system
5. Develop SBC application in tandem with web service
6. Test and analyse performance.

1 will involve determining which SBC to use, what development environment to use with it (e.g. some SBCs have different firmware which use different languages).

2 will require that a server-side data storage mechanism is decided on and implemented, this must be accessible by both the SBC and the graphing web service. A graphing web service must also be decided on and implemented. Security will be a concern for all these web services and must be explored and commented on.

3 will involve finding sensors that are capable interfacing with the SBC and have their data pulled as necessary.

4 will require interfacing with a solenoid driver to activate a pump for watering the plant.  This will involve designing a circuit layout in the form of a breadboard prototype.

5 will be the actual design of the software for the SBC as well as the setup of the server-side components, the database and graphing web service.

Finally, part 6 will involve testing and analysing performance, along with determining viability of solar power as an option. This will mean testing all the sensors for both accuracy and precision and testing the pump is operating within reasonable limits for both ml of water pumped and power usage. The power usage of the system with and without the pump will also be measured, to use for determining if solar power is a viable option.

# Ethics

Although the project itself has little ethical requirements due to its nature, I must still be ethical with regards to the report writing, making sure to reference all sources correctly and not claim credit for work that is not mine.

# Research

## Existing Solutions

A number of commercial plant monitors exist, one in particular being the Parrot Flower Power, by Parrot. This is a small unit, placed directly in the plants soil, which connects to a nearby phone via Bluetooth and reports on not only soil moisture level, temperature and light level but nutritional intake of the plant, powered with a 6 month battery. It also features a graph interface accessible via the app. [1]

This however has rather poor reviews on sites such as CNET [2] and is now discontinued. The system I am planning to develop will have a number of advantages over this product:

- Potentially Cheaper
- Autonomous
- Self-watering
- No need for phone interaction

## Hardware

Many SBCs exist that are suitable for this project, such as the Arduino Uno/Nano and clones, the ESP8266 and derivatives, the Raspberry Pi and a ton of Chinese clones. This project will involve further research into these and comparing their suitability across several factors, such as cost, GPIO and power usage.

The sensors used will be fairly standard, a soil moisture sensor, thermometer and light sensor. Many ultracheap sensors are available locally and from China. I will explore a number of available choices and compare them.

The pump system will need to be suitable for the plant. This will involve a reservoir, pump, power source and a method of activating the pump, such as a solenoid. I will need to research the types of pumps available and compare how much power they use, how much space they take up and how reliable they are.

## Software

Depending on the SBC chosen, there are a few software considerations to make. If choosing a full featured SBC such as the Pi, the entire software stack will be capable of running on it, however if something like an Arduino or ESP8266 is chosen, the data storage and graphing software must be installed on a server instead.

I will also need to research a suitable database to store information from the device, along with suitable software for presenting this info in a graph. This will mean I must compare the many  database solutions available, talk about the advantages and disadvantages of each and talk about how they tie in with the graphing software.

# Work Plan

## Timeline

1. **Research Components of Project                                                -        2 weeks**
  1.1. Research Hardware                                                        -        1 week
    1.1.1. Determine SBC to use
    1.1.2. Determine Moisture Sensor
    1.1.3. Determine LDR (or other optic sensor)
    1.1.4. Determine Thermometer
  1.2. Research Software                                                        -        1 week
    1.2.1. Determine firmware to use on SBC
    1.2.2. Determine Data Storage Solution to used
    1.2.3. Determine Graphing Web Service to use
2. **Design Hardware Implementation                                                -        2 weeks**
  2.1. Set up SBC and interface with sensors                                        -        1 week
    2.1.1. Set up each sensor individually and pull data from it
    2.1.2. Test pushing of data from each sensor
  2.2. Set up solenoid driver with SBC                                                -        1 week
    2.2.1. Test activation of solenoid driver
    2.2.2. Determine how long to run pump for desired output
3. **Design Software Implementation                                                -        2 weeks**
  3.1. Set up Data Storage Solution                                                -        1 week
    3.1.1. Implement DSS in appropriate fashion taking security into account
  3.2. Set up Graphing Web Service                                                -        1 week
    3.2.1. Implement Graphing Web Service, track statistics and infer data from them
4. **Testing and Refinement                                                        -        2 weeks**
  4.1.Test values for accuracy                                                -        1 week
    4.1.1. Use known working thermometer to measure temperature
    4.1.2. Test pump output matches expected values
  4.2. Determine Solar Power viability                                        -        1 week
    4.2.1. Measure power output and Sun                                        -        2 days
      4.2.1.1. Compare power usage to potential photovoltaic panel power generation
      4.2.1.2. Possibly use LDR to generate an estimate of power generation
    4.2.2. Research available solutions                                        -        3 days

# Conclusions

I believe with this preliminary report I have adequately outlined the steps I will need to take to fulfil this project in accordance with the guidelines set out by DIT. I have broken down the project into 6 steps, briefly talked about existing solutions and gave a timeline of events for the project.

# References

| [1] | Parrot, &quot;Parrot - Flower Power,&quot; 2017. [Online]. Available: [http://global.parrot.com/au/products/flower-power/](http://global.parrot.com/au/products/flower-power/). [Accessed 16 February 2017]. |
| --- | --- |
| [2] | A. Gebhart, &quot;Parrot Flower Power Review,&quot; 4 June 2014. [Online]. Available: [https://www.cnet.com/uk/products/parrot-flower-power/review/](http://global.parrot.com/au/products/flower-power/). [Accessed 16 February 2017]. |