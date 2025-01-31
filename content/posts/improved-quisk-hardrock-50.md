+++
slug = 'improved-quisk-hardrock-50'
title = 'Improved Hardrock-50 Widget for Quisk'
date = 2024-03-07T12:57:00+00:00
draft = false
tags = []
+++

Earlier this month I published a blog post about the [Hardrock-50 Widget for Quisk](/quisk-hardrock-50/) written by Harald, OE6HKE. Since I wanted to add some functionality, I've now forked his project, which can be found [here](/files/HR50-Quisk-Widget.tar.gz).

The first improvement is the error handling, so that the widget doesn't freeze Quisk when the backend script isn't running.

Also, the enhanced version now reads the selected band from Quisk, sends it to the backend script, which then switches the band filters inside the Hardrock-50 to match the active band of the Hermes Lite 2. This allows me to get rid of the serial cable between the Hermes Lite 2 and the Hardrock-50, which caused me a lot of problems in the past.
