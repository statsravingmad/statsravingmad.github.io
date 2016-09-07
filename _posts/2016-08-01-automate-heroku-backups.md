---
title: Automate Heroku PostgreSQL backups and downloads
author: manos_parzakonis
excerpt: Bash script to schedule and download heroku postgres database
comments: true
categories:
- automation
tags:
- bash
- script
- heroku
- postgres

---

{% include toc title="Unique Title" icon="file-text" %}

## Intro
For various reasons and under certain circumstances it can be useful to have the database in your local machine (e.g your Business Intelligence tool is incapable of incremental load and/or you need to have the lowest granularity of data in your reports etc). 
In that case, let's recap our setup :
- We use PostgreSQL  as our database
- We host our database on Heroku
- Our BI tool is located to our loc

## How to backup
You can backup in Heroku either using the GUI of the postgres.heroku.com or using the Heroku Toolbelt and its command line functionality. It is straightforward to create a backups using the GUI. However, you cannot perform any "automation" tasks on it. So, in the following we will use the Heroku Toolbelt . 

## Commands
**Note** : I am using Bash on Windows to run the script below. It is a habit of my nature to be a alpha/beta user of software :)
We will revolve around the pg:backups command of Heroku. In order to execute a command of the Heroku Toolbelt we need to start with the heroku  but first of all we need to login to Heroku

```bash
~# heroku login
Enter your Heroku credentials.
Email:_
```
Then you can run everything using heroku. Below we create a backup (`pg:backups capture`), schedule it (`pg:backups schedule`), download it (`curl`), attempt to restore it (`pg_restore`) and then manually run it.

``` bash
# !/bin/bash
# Script to create, schedule and download a database backup on Heroku
# ref to http://devcenter.heroku.com/articles/pgbackups for more info
# Contribution(s) by Alex Aslanoglou

# Some definitions to start with...
filename=heroku_db_$(date +%Y%m%d)_$(date +%H%M).dump
user=USERNAME
database=DATABASE

# Create a backup.
# Either leave this commented or comment it out if you are not feeeling like 
# scheduling using the schedule argument (see below)
# heroku pg:backups capture --app heroku-postgres-210c60 HEROKU_POSTGRESQL_NAVY --remote production

# Schedule backups
# heroku pg:backups schedule --app heroku-postgres-210c60 --at '12:00 Europe/Athens'

# Download latest database backup
curl -o $filename `heroku pg:backups --app heroku-postgres-210c60 public-url`

# Restore the database.
# It assumes that the database EXISTS already in your system!
# It also don't works until you pass the credentials to the connection, ref this discussion http://goo.gl/2VeUs.
# pg_restore --verbose --clean --no-acl --no-owner -h 127.0.0.1 -U $user -d $database -p 5432 -j 10 $filename

# End of Script. Execute it with something like this
# cd /mnt/c/Users/Manos/Documents/ && sudo ./heroku_bash.sh
```
You can schedule the script execution using either crontab or at provided you local machine stays on when the script is scheduled.
```bash
~# at now +24 hours -f heroku_backup.sh
```

Hope this helps you!
