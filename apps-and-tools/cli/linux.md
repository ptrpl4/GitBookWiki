# linux

## Built-in commands

## Basics

### apt-get

```bash
apt list --upgradable

apt-get update
apt-get

# not rm packages explicitly installed by the user
apt-get upgrade --autoremove 

# rm all unused packages
apt-get full-upgrade
```

### etc

```
# check OS version
cat /etc/os-release
```
### Cron

links:

- https://crontab.guru

```bash
# list of cronejobs
crontab -l

# edit  /var/spool/cron/crontabs 
crontab -e

# at midnight every Saturday
0 0 * * 6 /home/user/scripts/create_backup.sh >> /home/usr/logs/backup_log.txt
```

```bash
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of the month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday;
# │ │ │ │ │              7 is also Sunday on some systems)
# │ │ │ │ │
# │ │ │ │ │
# * * * * * <command to execute>
```

| **Special Character** | **Purpose**                                               | **Example**                                                  |
| --------------------- | --------------------------------------------------------- | ------------------------------------------------------------ |
| *                     | Match all possible values for the field                   | 0 18 * * * echo 'This runs every day at 6 pm'                |
| ,                     | Specify multiple values for the field separated by commas | 0 18 1,15 * * echo '1st and 15th day of every month at 6 pm' |
| -                     | Specify a range of values for a field                     | 0 18 * * 1-5 echo ' Monday to Friday of every week at 6 pm'  |
