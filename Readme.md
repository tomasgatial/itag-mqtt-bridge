# ITag MQTT Brigde

This project scans, automatically connects for nearby ITag devices and bridges their events to mqtt. 

This BLE to MQTT bridge enables you to use you smart BLE tags with any smart home solution which uses MQTT either your own custom one or e.g. [HomeAssistant](https://home-assistant.io/)

ITag is cheap <2€ BLE device equipped with button and piezzo buzzer. 

![](https://b4x-4c17.kxcdn.com/android/forum/data/attachments/36/36648-d31cf449cc2701c04a8b41e606ed0f19.jpg)

# Usage 
sudo is needed to build noble dependencies
``` bash
sudo yarn install 
yarn start
```
### Running without sudo  
See this [link](https://github.com/sandeepmistry/noble#running-without-rootsudo) 

## MQTT
- `itag/<tag uuid>/presence` emits values `1` or `0` if the device is connected or not
- `itag/<tag uuid>/button/click` emits `1` on button click
- `itag/<tag uuid>/alert/continuous` on payload: `< miliseconds >`  will perform a continuous piezzo aler for ms duration  
- `itag/<tag uuid>/alert/beep` on payload: `< miliseconds >` will perform a beeping piezzo aler for ms duration  

 
## Configuration 
Is done using [environment variales](https://en.wikipedia.org/wiki/Environment_variable`).
- `BEEP_ON_ITAG_CONNECT` default: `true` -> tag will beep after connecting to your computer
- `LOG_LEVEL` default: `debug` (see: [winston log levels](https://github.com/winstonjs/winston#logging-levels))
- `MQTT_BASE_TOPIC` default:`itag` 
- `MQTT_URL` default: `mqtt://localhost:1883` (see:[mqtt.js format](https://www.npmjs.com/package/mqtt#mqttconnecturl-options))
- `MQTT_USERNAME` default: `null`
- `MQTT_PASSWORD` default: `null`

# ITag BLE 
[ITag i bought on e-bay](http://www.ebay.com/itm/Hot-Cute-Tag-Tracker-Bag-Pet-Child-Wallet-Key-Finder-Bluetooth-GPS-Locator-Alarm/291963199014?ssPageName=STRK%3AMEBIDX%3AIT&var=590957204833&_trksid=p2057872.m2749.l2649)

Note: ITag has some other services and characteristics available, below are listed only interesting ones

### Services
- `1802` -> Immediate Alert [ Alert Level ]
- `1803` -> Link Loss [ Alert Level ]
- `ffe0` -> Button [ Click ]

### Characteristics
- `ffe1` -> Click [ notify ]
- `2a06` -> Alert Level [ read write ]
    - `0x00` -> no alert
    - `0x01` -> mild alert (continuous)
    - `0x02` -> high alert (beeping)

Sadly, the devices this project was tested on had no characteristics for battery level.

# Inspired by 
- [$2 Bluetooth Tags and Tangible UIs for IoT](https://medium.com/@monkeytypewritr/2-bluetooth-tags-and-tangible-uis-for-iot-47599869a7fb)