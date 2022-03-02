# esp-energyMonitor
Esp32 based module, 220V ac powered, energy monitor with two ct sensors for solar power and home power, and a LCD I2C display for visualization. Integrated with ESPhome in my domotic house based in Home Assistant.
Any possibility of improvement is accepted.

![alt text](/images/final-pcb.jpg)

## Table of contenets
* [General info](#general-info)
* [Hardware](#hardware)
* [Software](#software)
* [Pcb](#pcb)

# General info
This is an extreamily simple power monitor that I build for my house. It has two ct-sensors which controls the current of solar panels production and house consumption. These sensors are controlled by an esp32 board for the connectivity with wi-fi to home assistant thanks to Esphome software.
The board is powered with a 220v ac directel so no external transformer is needed.

I have also add a 16x2 lcd so that to have an immediate output in my electrical service panel.

You could use this board also if you want to use just on ct-sensor and maybe add it later on.

In the future I'm planning to add one other ct-sensor for conditioning power consuption and also to add an irrigation system for my garden as the current irrigation system controller is always inside the service panel.

# Hardware
The module is composed by:
* esp32
* sct 013 030 30A/1V as ct sensor, x2
* 16x2 lcd display with i2c interface
* 2.5mm jack connector with 3 pin, x2
* 10k ohm resistor, x4
* 75ohm resistor, x2
* 10uF capacitor, x3

To have the best readings, the 75ohm resistor should be changed based on the max current you are going to mesure with the ct sensors, according to the following formula present in [openenergymomitor](https://learn.openenergymonitor.org/electricity-monitoring/ct-sensors/interface-with-arduino) page:
```
Burden Resistor (ohms) = (AREF * CT TURNS) / (2√2 * max primary current)
```
I've used 75 ohm resistor as I'm not going to mesure currents higher than 26A.

Of course if you'd want just one ct sensor you should use 2x 10k ohm resistors, 75ohm resitor and 10uF capacitor less from the components.
I'm planning to make a version with the normal jack connector directly soldered in the pcb.

# Software

# Pcb

