+++
slug = 'qrzlogger'
title = 'qrzlogger'
date = 2021-07-13T00:00:00+00:00
draft = false
tags = ["Ham Radio", "Python", "Software"]
+++
I'm in the progress of developing a Python command line application to log QSOs directly into the QRZ.com logbook.

It does the following:

  * asks the user for a call sign
  * displays available call sign info pulled from QRZ.com
  * displays all previous QSOs with this call (pulled from QRZ.com logbook)
  * asks the user to enter QSO specific data (date, time, report, band etc.)
  * uploads the QSO to QRZ.com's logbook
  * fetches the just uploaded QSO from QRZ.com for review
  * starts again from 1)

## Code

You can find the code and more information [here](/files/qrzlogger.tar.gz).

## Screenshot


![](/img/qrzlogger-1.jpg)
