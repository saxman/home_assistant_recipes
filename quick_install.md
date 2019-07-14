# Home Assistant Quick Install on Raspbian

For my Home Assistant set-up, I chose to install HASS on a Raspberry Pi 3 B running Raspbian. Since I want this to be a stripped back Linux install just for HASS, and possible other home automation services, I installed the _Lite_ Raspbian version, which includes minimal tools and no window manager. These (tools and window manager) will be installed per recipe, as needed.

## 1. Set up Raspbian for (headless)
ref: Raspbian install process

## 2. Install HASS (manual install)
https://www.home-assistant.io/docs/installation/raspberry-pi/

Stop after you run "hass" for the first time, and have verified that the installation works.

```
(homeassistant) $ hass
```

http://raspberrypi.local:8123

## 3. Configure HASS to auto-start with Linux boot
https://www.home-assistant.io/docs/autostart/systemd/

Create the systemd config for HASS

```
$ sudo vi /etc/systemd/system/home-assistant@homeassistant.service
```

```
[Unit]
Description=Home Assistant
After=network-online.target

[Service]
Type=simple
User=%i
ExecStart=/srv/homeassistant/bin/hass -c "/home/%i/.homeassistant"

[Install]
WantedBy=multi-user.target
```

Restart systemd to pick-up the HASS service configuration and start the service.
```
$ sudo systemctl --system daemon-reload
```

Open a second terminal window and monitor the logs.
```
$ sudo journalctl -f -u home-assistant@homeassistant
```

Start the service.
```
$ sudo systemctl start home-assistant@homeassistant
```

If everything looks good, enable auto-starting for HASS when the system reboots.
```
$ sudo systemctl enable home-assistant@homeassistant
```

If necessary, the service can be stopped, restarted, and auto-starting diabled.
```
$ sudo systemctl stop home-assistant@homeassistant
$ sudo systemctl restart home-assistant@homeassistant
$ sudo systemctl disable home-assistant@homeassistant
```

These commands need to be run as user _pi_, not _homeassistant_, since the latter doesn't have sudo priveledges.

## 4. Start editing HASS configuration

Change to the HASS user, go to the HASS home directory, and enter the HASS directory.
```
$ sudo -u homeassistant -s
$ cd
$ cd .homeassistant
```

View the main HASS configuration file.
```
$ more configuration.yaml
```
