---
title: "Ewaso - Radio Mapping"
date: 2020-12-26
tags: [RSSI, Kerlink, Lorix, project ewaso, DeKUT-DSAIL, Gateway, LoRa, The Things Network ]
header:
    image: "assets/img/radio/pp2.jpg"
excerpt: "Project Ewaso - Radio Mapping at Ol-Pejeta Conservancy"
mathjax: true
classes: wide
---
## Introduction
The robust operation and efficient deployment of many IoT systems rely on the deployment of gateways and relays to ensure quality wireless coverage. Radio mapping aims to predict network coverage extent based on a small number of link measurements (signal properties) from sampled locations. We conducted radio mapping at Ol-Pejeta conservancy to determine transmission range of LoRa enabled (water level monitoring) prototypes developed, and also to test the deployment along river Ewaso Nyiro within the conservancy. We mounted two LoRaWAN gateways at Ol-Pejeta house, a Kerlink Gateway at approximately 16 meters above ground and a LoRix One Gateway at approximately 13 meters above ground.
{% include figure image_path="assets/img/radio/pp.PNG" alt="*Fig 1: Location of the [Ol-pejeta conservancy offices (gateway location)](https://www.google.com/maps/place/Ol+Pejeta+Conservancy/@0.0240155,36.9039278,15.72z/data=!4m6!3m5!1s0x0:0x9e73769f2ce0f944!4b1!8m2!3d-0.004314!4d36.963734)*" caption="*Fig 1: Location of the [Ol-pejeta conservancy offices (gateway location)](https://www.google.com/maps/place/Ol+Pejeta+Conservancy/@0.0240155,36.9039278,15.72z/data=!4m6!3m5!1s0x0:0x9e73769f2ce0f944!4b1!8m2!3d-0.004314!4d36.963734)*" %}
{% include figure image_path="assets/img/radio/pp1.PNG" alt="*Fig 2: Location of the [Ol-pejeta conservancy offices (gateway location)](https://www.google.com/maps/place/Ol+Pejeta+Conservancy/@0.0240155,36.9039278,15.72z/data=!4m6!3m5!1s0x0:0x9e73769f2ce0f944!4b1!8m2!3d-0.004314!4d36.963734)*" caption="*Fig 2: Location of the [Ol-pejeta conservancy offices (gateway location)](https://www.google.com/maps/place/Ol+Pejeta+Conservancy/@0.0240155,36.9039278,15.72z/d ata=!4m6!3m5!1s0x0:0x9e73769f2ce0f944!4b1!8m2!3d-0.004314!4d36.963734)*" %}
The LoRaWAN gateway, also known as the concentrator, is used to relay data packets between the end devices (nodes) and the network server via the internet. It communicates over multi-channels with multi-spreading factors (SF). With this technique, nodes communicate with the gateway using different channels and data-rates (DR) without pre-negotiation and enables the gateway to accommodate about 10000 end devices at a go.
## Sampled Locations
The map below shows all sample locations at Ol-pejeta conservancy and their RSSI values.

{% include figure image_path="assets/img/radio/pp2.PNG" alt="*Fig 3: Radio mapping sample locations*" caption="*Fig 3: Radio mapping sample locations*" %}

We deployed 3 devices (mdot1, mdot2, mdot3 (mdot-named after the Multitech mDot)) at various points within the conservancy. The devices and gateways were already connected to The Things Network and the network server was relaying radio propagation data (RSSI values for each device at five diffrent locations) to an InfluxDB10 database for storage, awaiting processing. 
{% include figure image_path="assets/img/radio/pp5.PNG" alt="*Fig 4: Radio propagation data (highlighted - signal properties)*" caption="*Fig 4: Radio propagation data (highlighted - signal properties)*" %}

Table below shows the The Mean RSSI (in dBm) for the Five (5) Test Locations for each Gateway.
{% include figure image_path="assets/img/radio/pp4.PNG" alt="*Fig 5: The Mean RSSI (in dBm) for the Five (5) Test Locations for each Gateway*" caption="*Fig 5: The Mean RSSI (in dBm) for the Five (5) Test Locations for each Gateway*" %}
## Results
### RSSI - The Received Signal Strength Indication
This refers to the signal power that is received in milliwatts (mW), and it is measured in dBm. How clear a receiver can “hear” from a sender can be measured using this value. Received signal strength indication, i.e., RSSI, is usually a negative value; hence, the signal is better when it is more positive (closer to 0). The value ranges of typical LoRa RSSI is -140 dBm to -30dBm.
### Discussion / Conclusion
For our radio mapping experiments, we computed the mean RSSI for each of the 5 test locations that were used.  At approximately 150m away from the gateway, the LORIX One gateway outperformed the Kerlink gateway by a margin of 10dBm as well as at the furthest distance (approximately 7.5km) by 13dBm dBm. The best strength was realized at the nearest test location and reduced as we moved to test locations far away from the gateway, as depicted by Figure 6.
{% include figure image_path="assets/img/radio/pp7.PNG" alt="*Fig 6: The Received Strength Plots for the 5 Test Locations.*" caption="*Fig 6: The Received Strength Plots for the 5 Test Locations.*" %}
Plots in Figure 5 provides a quick graphical examination of the RSSI for each of five (5) test locations for each gateway. Outlier RSSIs were highly realized in test location 2 and they are plotted as individual points, while none were realized at location 1 (nearest to the gateways). However, location 1 depicts the highest notable degree of dispersion (spread) and skewness for both gateways. There is a general non-linear variation of the median positions of the RSSI, usually determined by various parameters, which include free space loss, shadowing, reflection and transmission, diffraction, among others.
## Acknowledgements
- Dr Ciira Wa Maina - Director, Centre for Data Science abnd Artificial Intelligence (DSAIL) - Dedan Kimathi University of Technology
- Mr Nahshon Mokua - Dedan Kimathi University of Technology
- Mr Wamae Mathenge - Dedan Kimathi University of Technology
- Centre for Data Science abnd Artificial Intelligence (DSAIL) - Dedan Kimathi University of Technology