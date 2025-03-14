# linux

## folders

```sh
# executable files for system-wide installed applications.
ls /usr/bin

# desktop application launchers
ls /usr/share/applications

# user apps, tools
ls /usr/local/bin
ls /opt
```

## proceses

- `systemd` 

starts and stops services, manages dependencies, and provides logging through  `journalctl` command

## Built-in commands

## Basics

### apt

- `apt-get` lower-level tool that provides more granular control over package management (more used for scripts)
- `apt` higher-level tool that provides a more user-friendly interface and is designed to be easier to use

How it works

- The package list is updated from the repositories specified in the `/etc/apt/sources.list`
- The package list is updated to reflect the latest available packages and their versions
- The package list is stored in the `/var/lib/apt/lists/` directory

Flags

- `-y`: Answers "yes" to any prompts
- `--no-install-recommends`: Does not install recommended packages.
- `--no-install-suggests`: Does not install suggested packages.
- `--purge`: Deletes configuration files when removing a package.
- `--dry-run`: Simulates the command without actually running it.
- `--verbose`: Shows more detailed output from the command.
- `--quiet`: Shows less detailed output from the command.

```bash
sudo apt update

# check source files
cat /etc/apt/sources.list # main list

ls /etc/apt/sources.list.d/ # other lists

# installed packages
apt list --upgradable
apt list --installed
apt list --manual-installed

# get package info
apt show
apt depends
apt rdepends

sudo apt full-upgrade --simulate
sudo apt full-upgrade # upgrade and clean

sudo apt autoremove # rm unused packages
sudo apt clean # clean only cache

sudo apt autoremove && sudo apt clean # full clean

sudo apt purge  # remove package and its conf files
```

### etc

```bash
# check system info
uname -a
cat /etc/os-release
cat /proc/cpuinfo
cat /proc/meminfo

## network
nslookup apple.com # get ip, check current dns server

# check gateway wlan/cable, device wlan/cabel mac-address
nmcli -f IP4.GATEWAY device show wlan0
nmcli -f IP4.GATEWAY device show eth0
nmcli -f GENERAL.HWADDR device show wlan0
nmcli -f GENERAL.HWADDR device show eth0

# check ip
hostname -I

# ports 
## check open
sudo netstat -tlnp #tpc
sudo netstat -tlnp -A inet # filter ipv4
sudo ss -tlnp # similar to ^ but more info

# find info by port
sudo lsof -i :53 # one more similar tool

# disc
df -h # list of all mounted filesystems

ncdu # interactive disk usage

## procceses

# get proccess info
sudo systemctl status lighttpd

# find parent proccess
ps -p 2200 -o ppid

ps aux | grep systemd
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
