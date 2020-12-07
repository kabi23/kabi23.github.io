---
title: "Ewaso - Maxbotix MB1010 Test & Calibration"
date: 2020-12-07
tags: [ MB1010, Maxbotix, mbed, project ewaso, DeKUT-DSAIL, calibration, analog-output]
header:
    image: "assets/img/mb/mb2.jpg"
excerpt: "Project Ewaso - To establish the precision and accuracy of the MB1010 ultrasonic sensor."
mathjax: true
classes: wide
---

## Introduction

Project Ewaso water level monitoring prototype incorporated a Maxbotix MB1010 LV-Maxsonar-ELI Ultrasonic sensor shown on Firgure 1.
{% include figure image_path="assets/img/mb/mb12.jpg" alt="*Fig 1: Maxbotix MB1010 LV-Maxsonar-ELI Ultrasonic sensor*" caption="*Fig 1: Maxbotix MB1010 LV-Maxsonar-ELI Ultrasonic sensor*" %}
I carried out preliminary experiments to verify the outlined MB1010 detection range and also to establish the precision and accuracy of the sensor. With 2.5V - 5.5V power the MB1010 provides very short to long range detection and ranging in a very small package. The MB1010 detects objects from 0-inches to 254 inches(6.45 METERS) and provides sonar range information from 6-inches out of 254 inches with 1 -inch resolution. The interface output formats included are :

- Pulse width output.
- Analog voltage output.
- RS232 serial output.


I utilized the **Analog Voltage Output** as the interface output format in Project Ewaso. In this format, distance / detection range is converted to a voltage signal that can be read by a microcontroller and calibrated later. The MB1010 Pin3: AN - outputs an analog voltage signal with a scaling factor of (VCC/512)V per Inch. A supply of 5V yields ~ 9.8mV/inch and a 3.3V yields ~ 6.4mV/inch. The output is buffered and corresponds to the most recent range data.  
{% include figure image_path="assets/img/mb/mb3.png" alt="*Fig 2: MB1010 ANALOG (AN) pin (in red)*" caption="*Fig 2: MB1010 ANALOG (AN) pin (in red)*" %}

## Data Collection Procedure

- ### Programing the [Multitech mDot™] (https://www.multitech.com/brands/multiconnect-mdot).
Mbed enabled module such as the Multitech mDot used in this work can be programmed in various ways including using an online program compiler and the offline mbed command line interface (CLI). I used the online program compiler to compile the program used. Code used in this work is as shown below.

```cpp
    #include "mbed.h"
    static AnalogIn range_sensor(A1); //ULTRASONIC SENSOR
    Serial pc(USBTX,USBRX);

    int main (){
    
        while(1){
            uint16_t height;
            height = range_sensor. read_u16();
            float height_cm=(height*0.0198442452); 
    /*CALIBRATION
    A=(VCC/512)volts/inch
    3.7/512= 7.2266*10^-3 volts/inch
    an inch = 2.54 cm
    hence volts/cm= (7.2266*10^-3v/inch)/2.54cm= 2.8451*10^-3V/cm
    DECIMAL VALUE TO VOLTAGE CONVERSION
    D=(DECIMAL_VALUE)/(2^16-1)*3.7V
    D=DECIMAL_VALUE*0.0000564584 VOLTS
    HEIGHT_IN_CM = (DECIMAL_VALUE*0.0000564584 VOLTS)/2.8451*10^-3V/cm = DECIMAL_VALUE*0.0198440828
        */
            pc.printf("\rDEPTH:%f CM\n",height_cm);
            wait(3.0);
            }
        }

```
- ### Sensor calibration (Check out the comments on the code above)
1. Assuming the payload (HEX: 16E1) is in hexadecimal form - as an unsigned 16-bit integer
2. convert it to decimal form to get (DEC: 5857). 
3. The system runs on a VCC of about 3.7V hence, if 3.3V yields 6.4mV/Inch(on the datasheet), 3.7V should yield 7.2mV/Inch or (2.834mV/Cm)
4. Example in Figure 3 below. 
{% include figure image_path="assets/img/mb/mb5.PNG" alt="*Fig 3: MB1010 Calibration example*" caption="*Fig 3: MB1010 Calibration example*" %}

- ### Circuit block diagram (After programing) 
1. Connect the positive terminal of the battery to **Pin 1 of the Multitech Mdot** and **Pad 2 of the MB1010**
2. Connect the negeative terminal of the battery to  **Pin 10 or 12 of the Multitech Mdot** and **Pad 1 of the MB1010**
3. Connect **Pin 27 (A1) of the Multitech Mdot** to **Pin 3 (AN) of the MB1010**.
{% include figure image_path="assets/img/mb/mb6.PNG" alt="*Fig 4: Circuit diagram*" caption="*Fig 4: Circuit diagram*" %} {% include figure image_path="assets/img/mb/mb8.PNG" alt="*Fig 5: PCB design*" caption="*Fig 5: PCB design*" %}

## Experiment Results
- measured various distances - 4 measurements per distance, (**MB1010 limit 646cm**), as shown in Figure 6 below.
{% include figure image_path="assets/img/mb/mb9.PNG" alt="*Fig 6: Actual distance - Measured distance dataframe*" caption="*Fig 6: Actual distance - Measured distance dataframe*" %}
- Plot (Measured distance aganist Actual distance)
{% include figure image_path="assets/img/mb/mb11.PNG" alt="*Fig 7: Actual distance - Measured distance plot*" caption="*Fig 7: Actual distance - Measured distance plot*" %}

## Results/Conclusion
High precision and accuracy Ultrasonic sensor according to the data collected.

## Referenced works
1. [Maxbotix MB1010 LV-Maxsonar-ELI Ultrasonic sensor datasheet](https://www.maxbotix.com/documents/LV-MaxSonar-EZ_Datasheet.pdf) 
