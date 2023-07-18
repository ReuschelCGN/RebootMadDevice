# RebootMadDevice V3-rpi - DB version for RPi
Reboot ATV devices via ADB or PowerSwitch when device is not responding to MAD or RDM.

Only works with Python 3.6 and above.

After restarting MAD it will take about 5 minutes before data is usable. 

#### Preconditions:
```
Create a service user for RMD and grant rights for gpio usage:
- sudo adduser --system --home /opt/rmd --group rmd
- sudo adduser rmd gpio
```
#### Install:
```
Start bash as service user and install service and requirements:
- sudo -u rmd bash
- cd /opt/rmd
- git clone -b V3-rpi https://github.com/ReuschelCGN/RebootMadDevice.git .
- python3 -m venv venv
- . venv/bin/activate
- pip install -r requirements.txt --upgrade

Copy example config and adjust settings:
- cp config.ini.example config.ini
- nano config.ini
- cp devices.json.example devices.json
- nano devices.json

Try if everything works, deactivate venv, leave bash:
- deactivate
- exit
```

#### systemd service file for auto run/restart:
create a new file named: `rmd.service`
```
[Unit]
Description=RebootMadDevice
After=network.target

[Service]
User=rmd
Group=rmd
ExecStart=/opt/rmd/RMDclientDaemon.sh start
ExecReload=/opt/rmd/RMDclientDaemon.sh restart
Restart=on-failure
RemainAfterExit=yes

[Install]
WantedBy=multi-user.target
```
add rmd as new service to systemd:
```
Copy this file into /etc/systemd/system as root:
- sudo cp rmd.service /etc/systemd/system/rmd.service

Once this has been copied, you can attempt to start the service using the following command:
Important!!! (GPIO runs only with ROOT):
- sudo su
- sudo systemctl start rmd.service

Status systemctl Command:
- sudo systemctl status rmd.service

Stop it using following command:
- sudo systemctl stop rmd.service

When you are happy that its starts and stops your app, you can have it start automatically on reboot by using this command:
- sudo systemctl enable rmd.service
```

```
The systemctl command can also be used to restart the service or disable it from boot up!
- sudo systemctl restart rmd.service
- sudo systemctl disable rmd.service
```

#### Doing a manual reboot (e.g. for testing):
 
A manual reboot can be done with the ManualReboot.py script:
```
   ManualReboot.py -o <DEVICE_ORIGIN_TO_REBOOT>
or
   ManualReboot.py --origin <DEVICE_ORIGIN_TO_REBOOT>
```

#### ADD MAPPER RESTART SCRIPT:

To use the mapper restart option the config parameter TRY_RESTART_MAPPER_FIRST has to be true.
```
TRY_RESTART_MAPPER_FIRST = True
```
Next step is to add the "MAPPER_MODE" to the device.json like:
```
"MAPPER_MODE": "ATLAS"
```
Then a bash script with the commands has to be created in the folder. The name has to be:
```
restart<MAPPER_MODE_VALUE>.sh
```
It is possible to use different MAPPER_MODE on each device.


#### Features and supported hardware:
```
- Reboot every device one times within 24 hours (optional)
- RDM and MAD support
- support for status LED with WS2812 led stripe
- support for external status LED via websocket (https://github.com/FabLab-Luenen/McLighting)
- usable with PowerBoard (Link will follow)
- usable with external commands
- usable with web api like sonoff
- usable with snmp
- usable with gpio
- relay mode NC or NO
HINT to devices.json for SWITCH_MODE "GPIO":
SWITCH_VALUE use GPIO.BCM so its the GPIO number (not the pin number)
SWITCH_OPTION is NC or NO (NC = normally closed / NO = normally open) and False if pin is then without power (Relais is in standby)
```
#### Whats new:
```
- RDM support
- based on MAD/RDM database 
- timeout can be configured in config.ini
- next reboot of a device only after defined timeframe
- Discord Webhook support (without discord_webhook dependency)
- requirements for raspi
- ADB reboot optional
- manual reboot script for testing
- IP ban check for MAD backend and PTC
- auto run over systemd service
```
## License
See the [LICENSE](https://github.com/GhostTalker/RebootMadDevice/blob/master/LICENSE.md) file for license rights and limitations (MIT).
