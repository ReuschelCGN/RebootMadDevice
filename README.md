# RebootMadDevice V3-docker - Docker DB version for server
Reboot ATV devices via ADB or PowerSwitch when device is not responding to MAD or RDM.

Only works with Python 3.6 and above.

After restarting MAD it will take about 5 minutes before data is usable. 

#### Install:
```
Raspberry or Server:
- git clone -b V3-docker https://github.com/ReuschelCGN/RebootMadDevice.git
- copy config.ini.example to config.ini and adjust the values
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
- RDM support
- based on MAD/RDM database 
- timeout can be configured in config.ini
- next reboot of a device only after defined timeframe
- Discord Webhook support (without discord_webhook dependency)
- ADB reboot optional
- manual reboot script for testing
- IP ban check for MAD backend and PTC
```
## License
See the [LICENSE](https://github.com/GhostTalker/RebootMadDevice/blob/master/LICENSE.md) file for license rights and limitations (MIT).
