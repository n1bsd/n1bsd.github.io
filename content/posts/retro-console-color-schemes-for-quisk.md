+++
slug = 'retro-console-color-schemes-for-quisk'
title = 'Retro Console Color Schemes for Quisk'
date = 2024-07-25T19:25:00+00:00
draft = false
tags = ["Quisk", "Retro"]
+++
I'm a big fan of [Quisk](https://james.ahlstrom.name/quisk/), the SDR software I use for my Hermes Lite 2 SDR transceiver, but I am not the biggest fan of the available color schemes. The following screenshot shows the default theme:

![!](/img/retro-console-color-schemes-for-quisk-01.jpg)

I prefer dark color schemes, which is why I created the two new themes below. However, there is currently the restriction that the color settings for the following elements are hard-coded and therefore cannot be adjusted as described:
 
* Sliders
* Power Button

Please be also aware that it's generally not the best idea to alter the configuration file described below as it might be overwritten when updating the application. It might be a good idea to backup the original file before altering it and the modified file before performing an update.

## Amber Monochrome CRT

The following screenshot shows the "Amber Monochrome CRT" color scheme:

![!](/img/retro-console-color-schemes-for-quisk-02.jpg)

To achieve the above, find the code block beginning with _color_scheme_B_ in the file _quisk_conf_defaults.py_ and change it to

```
color_scheme_B = {
    'color_bg'            : '#0F0F0F', # Lower screen background
    'color_bg_txt'        : '#FFBF00', # Lower screen text color
    'color_graph'         : '#0F0F0F', # Graph background
    'color_config2'       : '#FF8000', # Color in tab row of config screen
    'color_gl'            : '#2F4F4F', # Lines on the graph
    'color_graphticks'    : '#CCCCCC', # Graph ticks
    'color_graphline'     : '#FFBF00', # Graph data line color
    'color_graphlabels'   : '#FFBF00', # Graph label color
    'color_btn'           : '#141414', # Button color
    'color_check_btn'     : '#865E2E', # Color of a check button when it is checked
    'color_cycle_btn'     : '#865E2E', # Color of a cycle button when it is checked
    'color_adjust_btn'    : '#865E2E', # Color of an adjustable button when it is checked
    'color_test'          : '#865E2E', # Color of a button used for test (turn off for tx)
    'color_freq'          : '#FF9933', # Background color of frequency and s-meter
    'color_freq_txt'      : '#000000', # Text color of frequency display
    'color_entry'         : '#2E2E2E', # Frequency entry box
    'color_entry_txt'     : '#FFBF00', # Text color of entry box
    'color_enable'        : '#FFBF00', # Text color for an enabled button
    'color_disable'       : '#FF0000', # Text color for a disabled button
    'color_popchoice'     : '#555555', # Text color for button that pops up a row of buttons
    'color_bandwidth'     : '#4B2E0F', # Color for bandwidth display
    'color_txline'        : '#FFBF00', # Vertical line color for tx in graph
    'color_rxline'        : '#FF8000', # Vertical line color for rx in graph
    'color_graph_msg_fg'  : '#FF8000', # Text messages on the graph screen
    'color_graph_msg_bg'  : '#1E1E1E', # Background of text messages on the graph screen
}

```
In the application itself, set the option _Config -> Radio -> Fonts -> Color_ to the value "B". To obtain an amber-colored waterfall, the code block starting with _waterfallPaletteB_ must be found and modified as follows:

```
waterfallPaletteB = (
    (0, 0, 0, 0),
    (32, 25, 20, 0),
    (64, 58, 40, 6),
    (96, 78, 50, 16),
    (128, 120, 85, 29),
    (160, 144, 100, 51),
    (192, 195, 140, 43),
    (224, 198, 150, 35),
    (255, 255, 191, 0)
)
```

Finally, set the option _Config -> Radio -> Fonts -> Waterfall_ to the value "B".

## Green Monochrome CRT

The following screenshot shows the "Green Monochrome CRT" color scheme:

![!](/img/retro-console-color-schemes-for-quisk-03.jpg)

To achieve the above, find the code block beginning with _color_scheme_C_ in the file _quisk_conf_defaults.py_ and change it to

```
color_scheme_C = {
    'color_bg'            : '#0F0F0F', # Lower screen background
    'color_bg_txt'        : '#00CC00', # Lower screen text color
    'color_graph'         : '#0F0F0F', # Graph background
    'color_config2'       : '#008000', # color in tab row of config screen
    'color_gl'            : '#2F4F4F', # Lines on the graph
    'color_graphticks'    : '#CCCCCC', # Graph ticks
    'color_graphline'     : '#00FF00', # graph data line color
    'color_graphlabels'   : '#00FF00', # graph label color
    'color_btn'           : '#141414', # button color
    'color_check_btn'     : '#005500', # color of a check button when it is checked
    'color_cycle_btn'     : '#005500', # color of a cycle button when it is checked
    'color_adjust_btn'    : '#005500', # color of an adjustable button when it is checked
    'color_test'          : '#005500', # color of a button used for test (turn off for tx)
    'color_freq'          : '#208B57', # background color of frequency and s-meter
    'color_freq_txt'      : '#00FF00', # text color of frequency display
    'color_entry'         : '#2E2E2E', # frequency entry box
    'color_entry_txt'     : '#00FF00', # text color of entry box
    'color_enable'        : '#00CC00', # text color for an enabled button
    'color_disable'       : '#FF0000', # text color for a disabled button
    'color_popchoice'     : '#555555', # text color for button that pops up a row of buttons
    'color_bandwidth'     : '#003300', # color for bandwidth display; thanks to WB4JFI
    'color_txline'        : '#00CC00', # vertical line color for tx in graph
    'color_rxline'        : '#00FF00', # vertical line color for rx in graph
    'color_graph_msg_fg'  : '#008800', # text messages on the graph screen
    'color_graph_msg_bg'  : '#1E1E1E', # background of text messages on the graph screen
}
```

In the application itself, configure set the option _Config -> Radio -> Fonts -> Color_ to the value "C". This color scheme works nicely together with the existing waterfall palette "C":

```
waterfallPaletteC = (
(0, 0, 0, 0),
(32, 0, 25, 25),
(64, 6, 58, 41),
(96, 16, 78, 43),
(128, 29, 120, 41),
(160, 51, 144, 35),
(192, 116, 141, 43),
(224, 195, 198, 35),
(255, 245, 99, 3)
)
```

Finally, set the option _Config -> Radio -> Fonts -> Waterfall_ to the value "C".



