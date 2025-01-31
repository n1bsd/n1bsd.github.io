+++
slug = 'pseudo-bidirectional-meter'
title = 'HA: Reading the energy fed into the grid from the meter'
date = 2023-10-06T09:58:34.624216+00:00
draft = false
tags = ["Home Assistant"]
+++
For some modern electricity meters there is the possibility to read the current electricity consumption and the meter reading via an optical sensor. Ready-to-buy sensors are available for this purpose, which can be attached directly on the meter's sensor using the built-in magnet.

![](/img/pseudo-bidirectional-meter-1.jpg)

This sensor is connected to an ESP8266 board, which then collects and processes the information and makes it available via Wi-Fi. This board has Tasmota installed as firmware in my case.

![](/img/pseudo-bidirectional-meter-2.jpg)

Home Assistant users who have a photovoltaic system but no two-way electricity meter can often unfortunately only read out the current electricity consumption, but not the energy fed into the grid. In the case of my meter, however, this value is positive when energy is taken from the grid and negative when energy is fed into the grid.

In this case, by defining two new sensors within Home Assistant, it is possible to split the determined value and thus create one sensor each for the energy drawn and for the energy fed into the grid.

These two pseudo sensors can be created by editing the file _~/config/configuration.yaml_ and add the following code block:

```yaml
template:
  - sensor:
      - name: "Entnahme aus dem Netz"
        unit_of_measurement: "W"
        state_class: measurement
        device_class: energy
        unique_id: gridin
        state: >
          {% if states('sensor.stromzaehler_aktuelle_wirkleistung') | float(0) > 0 %}
            {% set gridin = states('sensor.stromzaehler_aktuelle_wirkleistung') | float(0) %}
          {% else %}
            {% set gridin = 0 %}
          {% endif %}
          {{ gridin }}
        attributes:
          last_reset: '1970-01-01T00:00:00+00:00'

      - name: "Einspeisung ins Netz"
        unit_of_measurement: "W"
        state_class: measurement
        device_class: energy
        unique_id: gridout
        state: >
          {% if states('sensor.stromzaehler_aktuelle_wirkleistung') | float(0) < 0 %}
            {% set gridout = (states('sensor.stromzaehler_aktuelle_wirkleistung') | float | abs) %}
          {% else %}
            {% set gridout = 0 %}
          {% endif %}
          {{ gridout }}
        attributes:
          last_reset: '1970-01-01T00:00:00+00:00'
```
The above code is heavily inspired by the following thread: [(solved) Help getting a value from negative to positive in template (*-1)](https://community.home-assistant.io/t/solved-help-getting-a-value-from-negative-to-positive-in-template-1/411076)

You might want or need to adapt the following parameters:
  * name: change this to how you want your sensor to be named
  * unique_id: give both sensors a unique id
  * 'sensor.stromzaehler_aktuelle_wirkleistung': replace this with the name of the sensor that provides the current consumption (negative and positive) of the meter

The following is a screenshot from my dashboard showing from top to down:
  * Data delivered directly from the electricity meter sensor
  * Overview of the energy taken out of the grid
  * Overview of the energy fed into the grid

![](/img/pseudo-bidirectional-meter-3.jpg)
