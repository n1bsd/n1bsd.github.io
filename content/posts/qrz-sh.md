+++
slug = 'qrz-sh'
title = 'qrz.sh - a CLI QRZ.com query tool'
date = 2020-11-09T00:00:00+00:00
draft = false
tags = ["Bash", "CLI", "Ham Radio", "Software"]
+++
This script queries the QRZ.com callsign database and returns
the result to the command line. A XML subscription plan with
QRZ.com is required for full functionality.

You can download the script here: [qrz.sh.tar.gz](/files/qrz.sh.tar.gz)


![](/img/qrz-sh-1.jpg)


# Installation

* Copy the file _.qrz.conf_ into your home directory
* Copy the file _qrz.sh_ into a directory that is in you PATH variable
* Set the correct file permissions: _chmod u+x qrz.sh_

# Dependencies

* curl

# Configuration

Edit the file _~/.qrz.conf_ like this:

```
user=<your QRZ.com username - typically your callsign>
password=<your QRZ.com password - NOT your API key>
```

# Usage

```
# qrz.sh <callsign>
```
