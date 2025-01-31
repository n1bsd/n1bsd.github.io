+++
slug = 'mikrotik-ap-troubleshooting'
title = 'Troubleshooting a Mikrotik WiFi Access Point'
date = 2024-12-15T10:00:00+00:00
draft = false
tags = ["Mikrotik"]
+++

Recently, I encountered a weird issue where one of my four WiFi access points, a Mikrotik wAP AC, stopped functioning correctly despite being powered on. Here's how I uncovered the issue and what I learned along the way.

## The Setup
* All network devices are from Mikrotik, with WiFi centrally managed via CAPsMAN.
* The affected access point, a Mikrotik wAP AC, was powered via PoE from my server rack in the basement.
* Recently, I upgraded my router from a Mikrotik hEX to a L009, which offers PoE on Ethernet port 8.

## The Symptoms
We suddenly didn't have WiFi coverage in a certain area of the house anymore. The wAP AC which is repsonsible for this area and was in place and working for years, appeared powered on but failed to show up in the list of provisioned devices in CAPsMAN. This was odd because the other access points were functioning perfectly, and the setup had been stable before the router upgrade.

## Troubleshooting Steps

#### Reset and Reprovisioning Attempts

I followed the standard reset procedures multiple times:
  * Pressed reset for 5 seconds during boot to reset the configuration.
  * Pressed reset for 10 seconds during boot to attempt reprovisioning.

Unfortunately, neither approach worked.

#### Netinstall for a Fresh Start

Using a Windows 10 laptop, I performed a fresh firmware install via Netinstall. Even with the new configuration, the AP couldn't be provisioned in CAPsMAN.

#### Suspected Hardware Fault

I began to suspect the wAP AC was defective. To test this theory, I grabbed a spare Mikrotik HAP AC Lite, which had been flashed with alternative firmware.

So I reinstalled the Mikrotik firmware on the spare unit using Netinstall.  However, the same problem persisted: Also no provisioning of this device via CAPsMAN.

#### Examining Logs

The Mikrotik router logs revealed the Ethernet link for the AP was going up and down multiple times. However, I didn’t initially scrutinize the timestamps, assuming this was related to my frequent resets and reprovisioning attempts.

#### Investigating the CAPsMAN Configuration
Since I've recently swapped the main router and copied over the configuration from the old Mikrotik Hex to the new L009, I suspected a problem with the configuration - especially with the certificates used in CAPsMAN. After reading the wiki and crosschecking everything I came to the conclusion that the config should be ok.

#### Testing PoE with an Inline Adapter
Believing the built-in PoE of the new L009 router might be the issue, I inserted a PoE injector inline and connected the AP to a different non-PoE switch port. Yet, this approach yielded no improvement.

## The Eureka Moment
While fiddling with the ethernet cables in the server rack to add an external PoE injector, I discovered the culprit: a damaged patch cable. **The nose clip** of the cable between the router and the patch panel **was broken**, meaning the connection wasn’t fully secured. The cable was plugged in just enough to deliver power via PoE, but the data link was unstable. This explained the intermittent link status and provisioning failures.

## Resolution
Replacing the faulty patch cable immediately resolved the issue. Both the original wAP AC and the spare HAP AC Lite could be provisioned and operated as expected.

## What I've learned

* **Don’t Overlook Physical Connections:** Always verify physical connections when dealing with networking issues.
* **Logs Are Your Friends:** In hindsight, a closer look at the timestamps for the link status messages might have saved some time.
* **Methodical Testing:** Systematically swapping components and isolating variables—such as using a PoE injector—helped eliminate potential causes, even if the root cause was ultimately a physical issue. It would also have helped a lot if I would have tested the CAPs provisioning via an alternatve port.

