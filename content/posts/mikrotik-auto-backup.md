+++
slug = 'mikrotik-auto-backup'
title = 'MikroTik: Auto-Backup der Routerkonfiguration auf einen FTP-Server'
date = 2019-01-04T17:31:00+00:00
draft = false
tags = ["Mikrotik"]
+++


# Einführung

Folgend wird beschrieben, wie man seinen MikroTik Router automatisch sichert und das Backup auf einen FTP-Server überträgt. Zielsetzung ist die folgende:

  * Erstellung eines Exports und eines Backups
  * Speicherung des letzten Backups/Exports auf dem Router selbst
  * Übertragung der Dateien auf einen FTP-Server

# Vorbedingungen

  * MikroTik RouterBoard, z.B. ein MikroTik hEX
  * FTP-Server im LAN

# Quellen

  * [Mikrotik Forum Post von rextended](https://forum.mikrotik.com/viewtopic.php?t=87749#p440665)
  * [https://github.com/massimo-filippi/mikrotik/blob/master/backup-config.rsc](https://github.com/massimo-filippi/mikrotik/blob/master/backup-config.rsc)

# Los geht's

## Skript

    ## environment specific configuration
    :local ftpServer    "1.2.3.4"
    :local ftpUser      "user"
    :local ftpPassword  "pass"
    :local ftpPath      "/"
    
    :local dstFile      "backup"
    :local srcFile $dstFile
    :local myVer value=[/system package update get installed-version];
    :local id value=[/system identity get value-name=name];
    :local date value=[/system clock get date];
    
    ## replace "/" with "-" in date
    :local newdate value="";
    :if ([:find $date "/" -1] > 0) do={
     :for i from=0 to=([:len $date] -1) step=1 do={
      :local actualchar value=[:pick $date $i];
      :if ($actualchar = "/") do={ :set actualchar value="-" };
      :set newdate value=($newdate.$actualchar);
     }
    };
    
    ## construct destination file name
    :set dstFile ($dstFile . "-" . $id . "-" . $myVer . "-" . $newdate);
    
    ## perform the actual backup / export
    /system backup save name="$srcFile"
    /export file="$srcFile"
    
    ## upload both files via FTP
    :foreach i in=(".backup", ".rsc") do={
      /tool fetch address=$ftpServer src-path=($srcFile . $i) user=$ftpUser mode=ftp password=$ftpPassword dst-path=($ftpPath . $dstFile . $i) upload=yes port=21
    }
    
    ## Log
    :log info ("Configuration backup created on router $[/system identity get name].")


## Scheduler

Nachdem das Skript unter dem Namen "backup-config" angelegt wurde, kann man es mit folgendem Befehl täglich um 3:00 Uhr ausführen lassen:

    /system scheduler
    add interval=1d name=backup-config on-event=backup-config start-date=jan/05/2019 start-time=03:00:00

