# RebootMadDevice V3-docker - Docker DB version for server
Reboot ATV devices via ADB or PowerSwitch when device is not responding to MAD or RDM.

Only works with Python 3.6 and above.

After restarting MAD it will take about 5 minutes before data is usable. 

#### Install:
```
Server:
- git clone -b V3-docker https://github.com/ReuschelCGN/RebootMadDevice.git
- copy config.ini.example to config.ini and adjust the values
- copy devices.json.example to devices.json and adjust the values
- insert content of docker-compose.yml into your existing docker-compose.yml from mad/rdm... etc
- docker-compose build rmd
- docker-compose up -d rmd

- for logs: docker-compose logs -f -t rmd
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
```


#### Whats new:
```
- Add waittime for force reboots
- Add support to restart mapper software instead of reboot
- RDM support
- based on MAD/RDM database 
- timeout can be configured in config.ini
- next reboot of a device only after defined timeframe
- Discord Webhook support (without discord_webhook dependency)
- ADB reboot optional
- manual reboot script for testing
- IP ban check for MAD backend and PTC
- docker environment
```
## License
See the [LICENSE](https://github.com/GhostTalker/RebootMadDevice/blob/master/LICENSE.md) file for license rights and limitations (MIT).
