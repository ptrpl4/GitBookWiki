# 🍇 Raspberry Pi

links
- https://www.raspberrypi.com/documentation

## Commands

### Linux

```bash
# temperature check
vcgencmd measure_temp

# check gateway wlan/cabel, device wlan/cabel mac-address
nmcli -f IP4.GATEWAY device show wlan0
nmcli -f IP4.GATEWAY device show eth0
nmcli -f GENERAL.HWADDR device show wlan0
nmcli -f GENERAL.HWADDR device show eth0

# check ip
hostname -I

# check installed
apt list --installed
dpkg --get-selections

# apt-get update
sudo apt-get update
# check outdated
sudo apt-get -s dist-upgrade # --simulate

# ports 
## check open
netstat -lntu

```

### Pi

```bash
# manage settings
sudo raspi-config
```

## Connect

app - https://connect.raspberrypi.com
doc - https://www.raspberrypi.com/documentation/services/connect.html

```bash
# install. Connect automatically starts at login
sudo apt update
sudo apt full-upgrade
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

## homebridge

```bash
# home
cd /home/homebridge/

sudo hb-service stop # start / restart
sudo hb-shell # shell
```