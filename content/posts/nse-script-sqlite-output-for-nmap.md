+++
slug = 'nse-script-sqlite-output-for-nmap'
title = 'NSE-Script: SQLite output for Nmap'
date = 2014-05-05T00:00:00+00:00
draft = false
tags = ["Nmap", "Software"]
+++
I wrote this little NSE script that allows you to store the output of Nmap into a SQLite database: [nmap-sqlite-output.tar.gz](/files/nmap-sqlite-output.tar.gz)

This might come in handy when performing large inventory scans. The SQLite database can be queried and sorted easily or exported as a CSV file. This way you can, for example, easily generate tables for your assessment report.


## Example

```
$ nmap -sS -A -F --script sqlite-output scanme.nmap.org
[...]
$ sqlite3 scan.sqlite
[...]
sqlite> select * from scandata;
scanme.nmap.org|74.207.244.221|22|tcp|ssh|open|OpenSSH5.3p1 Debian 3ubuntu7.1
scanme.nmap.org|74.207.244.221|80|tcp|http|open|Apache httpd2.2.14
```
