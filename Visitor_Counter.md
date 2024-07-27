# Visitor Counter Project

## Table of Contents
1. Overview
2. Components required
3. Libraries needed to be installed
4. Module Develpment
5. Setting up Blynk application
6. Tests and Results
7. Video Demonstration
8. Conclusion

## Overview

<div align="justify">
This project enables real-time monitoring of the total number of incoming, outgoing, and current visitors. By utilizing Ultrasonic sensors, the system accurately counts and differentiates between incoming and outgoing visitors. The data is seamlessly uploaded to the Blynk cloud using the NodeMCU ESP8266 WiFi Module, allowing you to access and monitor the visitor data from anywhere in the world via the Blynk Dashboard.
</div>
<br>
<div align="justify">
The system uses two Ultrasoic sensors placed at the entrance to ensure accurate bidirectional counting. Sensor 1 is positioned at the beginning of the entrance, while Sensor 2 is placed slightly after Sensor 1 along the path of entry/exit. The counting logic is straightforward: an entry is detected when a person crosses Sensor 1 followed by Sensor 2, while an exit is detected when a person crosses Sensor 2 followed by Sensor 1. To ensure accurate counting, the system only registers a count when the sequence of sensor activations matches the predefined patterns for entry or exit, thereby preventing partial crossings (e.g., a person walking halfway through and then stepping back) from being counted.
</div>

## Components Required
<table>
  <tr align="center">
    <th>
      Sr. no.
    </th>
    <th>
      Component names
    </th>
    <th>
      Quantity
    </th>
  </tr>
  <tr align="center">
    <td>
      1.
    </td>
    <td>
      NodeMCU ESP8266
    </td>
    <td>
      1
    </td>
  </tr>
  <tr align="center">
    <td>
      2.
    </td>
    <td>
      HC-SR04 Sensors
    </td>
    <td>
      2
    </td>
  </tr>
  <tr align="center">
    <td>
      3.
    </td>
    <td>
      OLED Display
    </td>
    <td>
      1
    </td>
  </tr>
  <tr align="center">
    <td>
      4.
    </td>
    <td>
      Jumper Wires
    </td>
    <td>
      As per requirement
    </td>
  </tr>
  <tr align="center">
    <td>
      5.
    </td>
    <td>
      Breadboard
    </td>
    <td>
      1
    </td>
  </tr>
  <tr align="center">
    <td>
      6.
    </td>
    <td>
      Relay Module
    </td>
    <td>
      1
    </td>
  </tr>
</table>

<br>
<img src="Components.png">
<br>

## Libraries needed to be installed
1. ESP8266 library for the ESP8266 microcontroller board.
2. OLED Display Library: Adafruit SSD1306 and Adafruit GFX Library.
3. ESP8266WiFi Library
4. BlynkSimpleEsp8266 Library for connecting with the Blynk IoT platform.

## Module Development
### Contections
1. Connect the I2C pins (SDA & SCL) of 0.96" OLED Display  with the NodeMCU ESP8266 D2 and D1 respectively.
2. Interface the trigger pin and echo pin of ultrasonic sensor 1 to the D6 and D5 respectively of ESP8266.
3. Interface the trigger pin and echo pin of ultrasonic sensor 2 to the D8 and D7 respectively of ESP8266.
4. Connect a 5V Relay module to the D3 pin of ESP8266.

### Setting up Blynk Application
<p align="justify">We need to set up the Blynk app to receive the visitor counter data from the ESP8266 NodeMCU board. To set up Blynk Application, Download and install the application on smartphone. Open the app and create a new account using e-mail address.</p>

<ul>
  <li>Click on create new project.</li>
  <li>Provide the name of your project</li>
  <li>Choose NodeMCU ESP8266 Dev board</li>
  <li>Select connection type as WiFi, then click on create button</li>
  <li>They sent the Blynk authentication tolen to the email address (needed later in programming).</li>
</ul>
