+++
slug = 'mikrotik-capsman-setup'
title = 'MikroTik: Zentrales Management von WiFi Access Points mit CAPsMAN'
date = 2018-12-31T17:31:00+00:00
draft = false
tags = ["Mikrotik"]
+++

# Einführung

Folgend wird beschrieben, wie man mit CAPsMAN beliebig viele Wireless Access Points verwaltet und dynamisch provisioniert. Zielsetzung ist die folgende:

  * Zentrale Verwaltung aller APs im Haus (zunächst 3 APs, weitere sollen jederzeit ohne Aufwand hinzugefügt werden können)
  * 2 SSIDs, je eine SSID für 2,4 GHz und eine für 5 GHz
  * Jeder AP soll die gleiche Konfiguration erhalten
  * Umgebung soll beliebig erweiterbar sein, z.B. weitere SSID mit eigenem VLAN

# Vorbedingungen

  * MikroTik RouterBoard mit installiertem "wireless" Package, z.B. ein MikroTik hEX
  * 1-n MikroTik Wireless Access Points, z.B. wAP AC

# CAPsMAN konfigurieren

## Channels

Zu Beginn definieren wir zwei Channels:

    /caps-man channel
    add band=2ghz-g/n control-channel-width=20mhz extension-channel=disabled frequency=2437 name="2.4 GHz"
    add band=5ghz-n/ac name="5 GHz"

Hierbei ist folgendes zu beachten:

  * 2.4 GHz WLANs sollten idealerweise die Kanäle 1, 6 oder 11 nutzen. CAPsMAN verlangt die Angabe der jeweiligen Trägerfrequenz, diese kann [hier](https://www.elektronik-kompendium.de/sites/net/1712061.htm) nachgeschaut werden (hier 2437 MHz = Kanal 6).
  * Bei einem Frequenzbereich von 2.4 GHz ist eine Kanalbreite von 20 MHz ideal
  * Auf 802.11g sollte man unbedingt verzichten, da dies die Gesamtgeschwindigkeit reduziert und sowieso keine g-Clients zu erwarten sein sollten.

## Security Configurations

In meiner Umgebung verwende ich das gleiche Passwort für beide SSIDs:

    /caps-man security
    add authentication-types=wpa2-psk disable-pmkid=yes encryption=aes-ccm group-encryption=aes-ccm group-key-update=1h name="Wifi Security Settings" passphrase=GeheimesPasswort

Hierbei ist folgendes zu beachten:

  * Auf WPA kann man in den meisten fällen verzichten, WPA2 ist immer zu bevorzugen
  * Mit "disable-pmkid=yes" mitigiert man die Schwachstelle CVE-2018-3580

## Datapaths

Hier gibt es kaum etwas zu beachten, gegebenenfalls muss anstelle von "bridge1" der Name der Bridge angegeben werden, die auf dem Router definiert wurde:

    /caps-man datapath
    add bridge=bridge1 client-to-client-forwarding=yes local-forwarding=yes name=BridgeDP

## Configurations

Die bisher definierten Einstellungen werden nun in zwei übergeordneten Konfigurationen eingesetzt. Diese Konfigurationen werden in einem späteren Schritt verwendet, um die APs zu provisionieren:

    /caps-man configuration
    add channel="2.4 GHz" country=germany datapath=BridgeDP installation=indoor name="2.4 GHz" security="Wifi Security Settings" ssid="Mein WLAN (2.4 GHz)"
    add channel="5 GHz" country=germany datapath=BridgeDP name="5 GHz" security="Wifi Security Settings" ssid="Mein WLAN (5 Ghz)"

## Provisioning

Folgende Einstellungen sorgen dafür, dass jeder AP vollständig dynamisch provisioniert wird:

    /caps-man provisioning
    add action=create-enabled hw-supported-modes=g master-configuration="2.4 GHz" name-format=prefix-identity name-prefix=2.4GHz
    add action=create-enabled hw-supported-modes=ac master-configuration="5 GHz" name-format=prefix-identity name-prefix=5GHz

Über den Parameter "hw-supported-modes" wird gesteuert, dass Interfaces, die "g" unterstützen (und es sich somit um 2.4 GHz Interfaces handelt), die Konfiguration "2.4 GHz" erhalten. 5 GHz Interfaces, welche "ac" unterstützen, erhalten wiederum die Konfiguration mit dem Namen "5 GHz".

## Manager

Schlussendlich aktivieren wir den Manager mit

    /caps-man manager
    set enabled=yes

# Inbetriebnahme

Zu beachten ist, dass man keine CAP Interfaces anlegen muss - dies geschieht vollautomatisch. Jetzt können die neuen APs hinzugefügt werden:

  * Während des Einschaltens 10 Sekunden lang Reset gedrückt halten bis die "AP/CAP"-LED anfängt zu blinken (von Modell abhängig)
  * Nach einer Weile erscheinen dann pro AP zwei CAP Interfaces mit dem Namensmuster "2.4GHz-MikroTik-1" bzw. "5GHz-MikroTik-1"
  * Fertig!

Sinnvolle ToDos:

  * Alle APs aktualiseren (Packages + Firmware)
  * Jedem AP ein Passwort vergeben
  * Jedem AP eine Identity geben, z.B. "AP EG", "AP OG" und "AP Büro" statt "MikroTik"
