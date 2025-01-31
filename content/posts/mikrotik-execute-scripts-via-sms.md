+++
slug = 'mikrotik-execute-scripts-via-sms'
title = 'MikroTik: Ausführen von Skripten per SMS'
date = 2018-12-30T17:31:00+00:00
draft = false
tags = ["Mikrotik"]
+++

# Einführung

Dieser Blogeintrag beschäftigt sich mit dem Ausführen von Skripten per SMS-Kommandos. In diesem Beispiel soll als Aktion eine SMS zurückgesendet werden, welche die aktuelle öffentliche IP-Adresse enthält.

# Vorbedingungen

  * MikroTik RouterBoard mit USB-Port, z.B. ein MikroTik hEX
  * USB UMTS-Stick, z.B. ein Standard Huawei Stick aus verschiedenen Prepaid-Angeboten

# Los geht's

Zuerst schauen wir nach, ob der Router den USB-Stick erkannt hat:

    [admin@hEX] /system resource usb> print
     # DEVICE VENDOR                                  NAME                                SPEED                               
     0 1-0    Linux 3.3.5 xhci-hcd                    xHCI Host Controller                480                                 
     1 2-0    Linux 3.3.5 xhci-hcd                    xHCI Host Controller                5000                                
     2 1-1    HUAWEI Technology                       HUAWEI Mobile                       480       
    

Anschließend versenden wir eine Test-SMS:

    /tool sms send usb1 "+4915212345678" message="Test"

Nach dem erfolgreichen Test kann das benötigte Skript wie folgt erstellt werden:

    /system script
    add dont-require-permissions=yes name=extip owner=admin policy=ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon source=\
        "/tool fetch url=\"https://icanhazip.com/\" mode=http dst-path=mypublicip.txt\r\
        \n:local currentIP [/file get mypublicip.txt contents]\r\
        \n:put \$currentIP\r\
        \n/tool sms send usb1 \"+4915212345678\" message=\"\$currentIP\";"
    
Zur Ermittlung der öffentlichen IP wird absichtlich nicht das WAN-Interface abgefragt, sondern ein externer Dienst, da mein Router nicht direkt mit dem Internet verbunden ist. Die Antwort des versendeten GET-Requests wird in der Datei mypublicip.txt zwischengespeichert und direkt darauf per SMS an die im Skript hinterlegte Nummer gesendet.

Zuletzt muss noch der Empfang von SMS aktiviert werden:

    /tool sms
    set allowed-number=+4915212345678 port=usb1 receive-enabled=yes secret=GeheimesPasswort

Wenn alles geklappt hat, muss man folgenden Text (ohne Anführungszeichen) and die Nummer des UMTS-Sticks senden:

    ":cmd GeheimesPasswort extip"

Als Antwort sollte anschließend die öffentliche IP-Adresse des Routers kommen.

Weitere Informationen sind dem MikroTik Wiki entnehmbar:

  * [Manual:Tools/SMS](https://wiki.mikrotik.com/wiki/Manual:Tools/Sms)
  * [Manual:Tools/Fetch](https://wiki.mikrotik.com/wiki/Manual:Tools/Fetch)
  * [Manual:Scripting](https://wiki.mikrotik.com/wiki/Manual:Scripting)
