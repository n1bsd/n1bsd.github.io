+++
slug = 'server-backup-with-borgmatic-and-borgbase'
title = 'Server Backup with Borgmatic and Borgbase'
date = 2021-11-18T00:00:00+00:00
draft = false
tags = ["Backup", "Linux"]
+++
In this article I document how I backup my servers locally as well as remotely to [borgbase.com](https://www.borgbase.com). After implementing the follwoing steps, you'll get:

  * encrypted backups to a local directory
  * encrypted offsite backups
  * alarms in the case backups fail

## Preconditions

All these steps have been tested with Debian 10/11. You'll need a paid subscription with BorgBase or use your own remote repository server. All commands have to be executed in the context of the user _root_.

## Installation of Borg and Borgmatic

On the system that needs to be backed up:

```
# apt install borgbackup borgmatic
# mkdir -p ~/.config/borgmatic
# cat ~/.ssh/id_rsa.pub
```

Copy the output of the last command. If you don't have SSH keys generated, you can do so by typing

```
# ssh-keygen
```

## Configuring the Repositories

Login to BorgBase (https://www.borgbase.com) and perform the following steps:

  * Add a new SSH key under _Account - SSH Keys - Add public key_ and paste here your previously copied public key
  * Go to _Repositories - New Repo_, create a new repository and add the new SSH pubkey
  * Look up how to initialize your repo under _Setup - Initialize Repo_. Copy the first command, e.g. "borg init -e repokey-blake2 abcdefgh@abcdefgh.repo.borgbase.com:repo"


## Initializing your Repositories

Now is the time to generate and store a secure passphrase in your password manager. This will be needed to encrypt/decrypt your backups

Initialize both the local and the remote repository by entering the following command. If asked, enter the previously generated passphrase:

```
# borg init -e repokey-blake2 /backup/borgmatic/
# borg init -e repokey-blake2 abcdefgh@abcdefgh.repo.borgbase.com:repo
```

## Configuration of Borgmatic

Create and edit the configuration file:

```
# vi ~/.config/borgmatic/config.yaml
```


Paste and adapt the following content:

```
location:
    source_directories:
        - /etc
        - /home
        - /var/www

    one_file_system: true
    repositories:
        - /backup/borgmatic
        - abcdefgh@abcdefgh.repo.borgbase.com:repo

    exclude_patterns:
        - '*.pyc'
        - ~/*/.cache
        - NoSyncNoBackup

    exclude_caches: true
    exclude_if_present: .nobackup

storage:
    compression: auto,zstd
    encryption_passphrase: <YOURSECRETPASSWORD>
    archive_name_format: '{hostname}-{now}'

retention:
    keep_daily: 3
    keep_weekly: 4
    keep_monthly: 12
    keep_yearly: 2
    prefix: '{hostname}-'

consistency:
    checks:
        # uncomment to always do integrity checks. (takes long time for large repos)
        #- repository
        - disabled

    check_last: 3
    prefix: '{hostname}-'

hooks:
    # List of one or more shell commands or scripts to execute before creating a backup.
    before_backup:
        - echo "`date` - Starting backup"

    after_backup:
        - echo "`date` - Finished backup"
```

Finally execute the following command to create your first backup:

```
# borgmatic
```

## Backup Scheduling

In order to automatically perform backups e.g. every day at 1:00 am, edit the crontab with

```
# crontab -e
```

Paste the following line into the editor:

```
0 01 * * * /usr/bin/borgmatic >/dev/null 2>&1
```