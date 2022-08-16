# Esp energy monitor - Version 1
### Soon will arrive the new and better [version](https://github.com/zioCristia/esp-energy-monitor-v1) of this board
Esp32 based module, 220V ac powered, energy monitor with two ct sensors for solar power and home power, and an LCD I2C display for visualization. Integrated with ESPhome in my IoT house based in Home Assistant.
Any possibility of improvement is accepted.

You can find a [video tutorial](https://www.youtube.com/watch?v=UY-CO4Zc2r0) (in italian) of this pcb on YouTube.

![alt text](/images/final-pcb.jpg)

## Table of contents
* [General info](#general-info)
* [Hardware](#hardware)
* [Software](#software)
* [Pcb](#pcb)

# General info
This is an extremely simple power monitor that I built for my house. It has two ct-sensors which control the current of solar panels production and house consumption. These sensors are controlled by an esp32 board for the connectivity with wi-fi to home assistant thanks to Esphome software. 
The board is powered with a 220v ac, so no external transformer is needed. 

I have also added a 16x2 lcd so that to have an immediate output in my electrical service panel.

You could use this board also if you want to use just on ct-sensor and maybe add it later on. The total cost for a board with two ct sensors (included) is around 20/25€.

In the future I'm planning to add one other ct-sensor for conditioning power consumption and to add an irrigation system for my garden as the current irrigation system controller is near the service panel.

![alt text](/images/pcb-installed.png)

# Hardware
The module is composed of:
* esp32
* sct 013 030 30A/1V as ct sensor, x2
* 16x2 lcd display with i2c interface
* 3.5mm jack connector with 3 pin, x2
* 10k ohm resistor, x4
* 75ohm resistor, x2
* 10uF capacitor, x3

To have the best readings, the 75ohm resistor should be changed based on the max current you are going to measure with the ct sensors, according to the following formula present in [openenergymonitor](https://learn.openenergymonitor.org/electricity-monitoring/ct-sensors/interface-with-arduino) page:
```
Burden Resistor (ohms) = (AREF * CT TURNS) / (2√2 * max primary current)
```
I've used 75 ohm resistor as I'm not going to measure currents higher than 26A.

Of course, if you'd want just one ct sensor and you should use 2x 10k ohm resistors, 75ohm resistor and 10uF capacitor less from the components.
I'm planning to make a version with the normal jack connector directly soldered in the pcb.

You can find some links of the components in the following [link](https://docs.google.com/spreadsheets/d/1uGH7hN6ZSQ5MoKFjuWFCn1lEjyLJ1oxg2DWpdToigKc/edit?usp=sharing).

# Software
The software used is EspHome in order to integrate the sensor with my HomeAssistant server but you may as well use another software to your liking.

You can find an example of the code I used [here](https://github.com/zioCristia/esp-energy-monitor-v1/blob/main/energy-monitor.yaml.example) or check in the [openenergymonitor](https://learn.openenergymonitor.org/electricity-monitoring/ct-sensors/how-to-build-an-arduino-energy-monitor-measuring-current-only?redirected=true) site for others examples.

The filter section is used to obtain the right value of current from the sensor. I've done it confronting the values read with a well know source (like a phone or an electric heater).
Moreover I've added a part to read zero values when we are near zero due to some interference that never allows to have values equal to zero.
```
filters:
      - calibrate_linear:
          # Measured value of 0.0065 maps to 0A
          - 0.0065 -> 0
          # Known load: 13.783A
          # Value shown in logs: 0.2348A
          - 0.2348 -> 13.783
      - lambda: |-
          if (x > 0.2) return x;
          else return 0;
```

I've added a software sensor which gives me the difference between the in power and the out power with which I do some automation in home assistant and allows me to maximize the usage of current produced by the solar panels.

The toGridEnergy and the toGridPower is used to interface with the new energy management feature in home assistant.

The lcd prints the solar power as IN, home power as OUT, the hour in the top right and below the difference between IN-OUT, so the differencePower.

# Pcb
The pcb layout is extremely simple with enough space for all the parts. The components are all through hole and the female jack connector is with 3 pins. Anyone can make this pcb at home with just a soldering iron. 

![alt text](/images/pcbLayout.png)

In the future I will change the type of the jack connectors with the ones more available in the online stores.
