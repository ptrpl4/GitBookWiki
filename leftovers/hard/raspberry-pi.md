# 🍇 Raspberry Pi

links
- https://www.raspberrypi.com/documentation

## Commands

### Linux

Moved to [linux](../../apps-and-tools/cli/linux.md)

### Pi

```bash
# manage settings
sudo raspi-config

# memory allocation
vcgencmd get_mem 

# temperature check
vcgencmd measure_temp

vcgencmd measure_volts
vcgencmd get_mem gpu

# watcher
watch -n 1 vcgencmd measure_temp
```

## Connect

app - https://connect.raspberrypi.com
doc - https://www.raspberrypi.com/documentation/services/connect.html

```bash
sudo apt update # fetch updates

# install connect
sudo apt install rpi-connect

sudo reboot

# get url to connect
rpi-connect signin

# off screen sharing
rpi-connect vnc off

# manual start
systemctl --user start rpi-connect

# manual reload
systemctl --user daemon-reload

# status
systemctl --user status rpi-connect
rpi-connect status
```

## soft

### pi-hole

links 
- https://pi-hole.net/
- https://www.raspberrypi.com/tutorials/running-pi-hole-on-a-raspberry-pi/

```bash
curl -sSL https://install.pi-hole.net | bash
```

### Home Assistant Core

install steps - https://www.home-assistant.io/installation/raspberrypi#install-home-assistant-core

```bash
# run h-a core
sudo -u homeassistant -H -s
cd /srv/homeassistant
python3 -m venv .
source bin/activate
```

### homebridge

```bash
# home
cd /home/homebridge/

sudo hb-service stop # start / restart
sudo hb-shell # shell
```

### tailscale

`tailscaled` - daemon

```bash
tailscale status
```
