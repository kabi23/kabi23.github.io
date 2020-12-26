---
title: "Project Ewaso (project overview)"
date: 2020-11-09
tags: [machine learning, IoT, water-level, deployment, DeKUT-DSAIL, anomaly-detection]
header:
    image: "assets/img/nyiro6.jpg"
excerpt: "Leveraging Machine Learning and IoT for Improved Monitoring of Water Resources"
mathjax: true
classes: wide
---
## Introduction
The [Upper Ewaso Nyiro](https://www.google.com/maps/place/0°02'46.5"N+36°54'09.2"E/@0.046248,36.9003563,17z/data=!3m1!4b1!4m6!3m5!1s0x0:0x0!7e2!8m2!3d0.0462484!4d36.9025452) (Ngare Ngiro) is one of the major rives in Kenya. For a millennium, it has been a lifeline for farmers and pastoralist in Kenya. All along its path, unfair distribution of water has fundamentally been the cause of conflict between and amongst the water-user communities over the years. Also, increased demand for farming water, reduced rainfall due to climate change and catchment degradation have led to reduced supply of the vital resource which in turns leads to the said conflict. To ensure equitable distribution of the water, effective monitoring of the river is essential. Monitoring is also necessary when sourcing data to quantify unsustainable water usage or destruction of the catchment area.

Ngare Ngiro also happens to be the only river running through the [Ol-Pejeta conservancy](https://www.olpejetaconservancy.org), which is located within the Ewaso Nyiro basin in Kenya. The conservancy is home to several species of wildlife including lions, rhinos, buffalos and zebras. The river is therefore a very vital part of [Ol-Pejeta](https://www.olpejetaconservancy.org) ecosystem and monitoring the status of water in it is important.

{% include figure image_path="assets/img/oll1.PNG" alt="*River Ewaso Nyiro at diffrent sections*" caption="*River Ewaso Nyiro at diffrent sections.*" %}

## Monitoring Systems Development
Since 2018, I have been developing appropriate systems for remote monitoring of water resources and have deployed prototypes along the Ewaso Nyiro river channel, as I continue to improve my systems. The hope is that I will be able to develop low cost devices capable of large scale deployment along river courses. Access to data collected by these prototypes/systems on river parameters such as river height, flow-rate and turbidity could then be used to ensure sustainable use of the water and protection of the catchment.

My initial/first design was based on the [Multitech mDot™](https://www.multitech.com/brands/multiconnect-mdot), which is an Arm® Mbed™ programmable LoRa module. The design also incorporated a [Maxbotix](https://www.maxbotix.com/Ultrasonic_Sensors/MB1010.htm) ultrasonic sensor for river water-level measurement and a custom PCB I designed and etched at DeKUT-DSAIL labs to house all the components. The design was deployed along River Ewaso Nyiro at Ol-Pejeta conservancy which is also home to a Wildlife Techlab which was setup to develop and test conservation technology. To avoid damage by primates in the conservancy, I decided to design a metallic cage to house and secure the prototype and have been receiving river water-level data since early May 2020.


{% include figure image_path="assets/img/oll2.PNG" alt="*The prototype etched onto a PCB.*" caption="*The prototype etched onto a PCB.*" %}
{% include figure image_path="assets/img/oll3.PNG" alt="*Systems deployed at the Ol Pejeta conservancy. The initial system (left) was destroyed by baboons. Our current system is protected by a cage (right).*" caption="*Systems deployed at the Ol Pejeta conservancy. The initial system (left) was destroyed by baboons. Our current system is protected by a cage (right).*" %}

## Power Analysis
Many IoT applications rely on small, rechargeable batteries, hence, optimizing the power available is very crucial for various reasons, the most obvious being that a long operating life is needed. In the most recent design, I added power analysis sensors to the first design to help in determining the amount of power the system is consuming and also the power coming in from the solar panel to charge the battery (`Solar Voltage * Solar Current`). The power analysis sensors include:
1.	Solar voltage sensor
2.	Solar current sensor
3.	Battery voltage sensor

{% include figure image_path="assets/img/oll4.PNG" alt="*Second prototype with the power analysis sensors(right). Test deployment(left).*" caption="*Second prototype with the power analysis sensors(right). Test deployment(left).*" %}

Currently, I am able to draw meaningful conclusions from the comparison of the solar power data being collected by the sensors to the short-wave radiation data from [TAHMO Weather Stations](https://tahmo.org).

{% include figure image_path="assets/img/oll5.PNG" alt="*Solar power data(top) compared to the short-wave radiation data(bottom) from TAHMO.*" caption="*Solar power data(top) compared to the short-wave radiation data(bottom) from TAHMO.*" %}

In addition to sensor development, I have developed web applications for data visualization and analysis. I have also develop machine learning methods to deal with anomalous sensor measurements and make predictions. In future I plan to incorporate multiple data sources such as weather and satellite imagery to build water-level prediction models.

{% include figure image_path="assets/img/oll6.PNG" alt="*Water-level anomaly detection and water level mean.*" caption="*Water-level anomaly detection and water level mean.*" %}

## Acknowledgement
I would like to thank William Njoroge and the management at Ol Pejeta for allowing me to test my systems at the conservancy and allowing me to use the Tech Lab to prepare and test our systems. I also thank the wardens at Ol Pejeta for help with the deployment. I thank ARM for support acquiring hardware and for technical support. Also, Stephen Wamae from DeKUT has been a great help during sensor development and deployment. I would also like to acknowledge TAHMO for allowing me to use their weather station data-streams.







