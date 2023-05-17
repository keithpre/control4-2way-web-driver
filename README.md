# control4-2way-web-driver

This driver allows you to get and set the state of any control4 device by making
an http request. The response is in JSON format.  This fork is a small extension to https://github.com/itsfrosty/control4-2way-web-driver to allow for programatic device discovery (getdevices and getvariables2)

Use Case:
---------
I am using this (along with my homebridge-c4-plugin) to connect homekit to my
control4. So I can say "Siri turn on Kitchen Lights" or "Siri is my garage door
closed". You can use this with any other hub which supports making http requests.

Examples:
----------
Commands:
- get
-- Get the current state of a variable for a device by proxy id (Can pass multiple ids in comma seperated list)
-- example- http://10.0.1.70:9000/?command=get&proxyID=30&variableID=1001
-- Result - {"1001":"0","success":"true"}
-- This result means the dimmer light with proxyID 30 is currently at 0%
- set
-- Get the current state of a variable for a device by proxy id
-- example- http://10.0.1.70:9000/?command=set&proxyID=30&variableID=1001&newValue=100
-- Result - {"success":"true"}
-- This set dimmer light with proxyID 30 to 100%
- getvariables
-- Get list of all variables with name and IDs for a device:
-- example- http://10.0.1.70:9000/?command=getvariables&proxyID=25
-- Result - Result: {"1005":"MAX_ON_LEVEL","1006":"MIN_ON_LEVEL","1003":"CLICK_RAMP_RATE_DOWN","1007":"START_ON_LEVEL","1000":"LIGHT_STATE","1004":"PRESET_LEVEL","1008":"START_ON_TIME_MS","1001":"LIGHT_LEVEL","1002":"CLICK_RAMP_RATE_UP","success":"true"}
- getvariables2
-- Get list of all variables for a device along with info about them (Similar to getvariables, but more detail and different structure)
-- example- http://10.0.1.70:9000/?command=getvariables2&proxyID=25
-- Result - {{"description":"","deviceid":"95","hidden":"0","name":"SCALE","readonly":"1","type":"1","value":"F"},"1101":{"description":"","deviceid":"95","hidden":"1","name":"TEMPERATURE","readonly":"1","type":"2","value":"221"},"1102":{"description":"","deviceid":"95","hidden":"1","name":"HEAT_SETPOINT","readonly":"0","type":"2","value":"177"},"1103":{"description":"","deviceid":"95","hidden":"1","name":"COOL_SETPOINT","readonly":"0","type":"2","value":"266"},"1104":{"description":"","deviceid":"95","hidden":"0","name":"HVAC_MODE","readonly":"1","type":"1","value":"Off"},"1105":{"description":"","deviceid":"95","hidden":"0","name":"FAN_MODE","readonly":"1","type":"1","value":"Auto"},"1106".....}
- getdevices
-- Get list of all variables for a device along with info about them (Similar to getvariables, but more detail and different structure)
-- example- http://10.0.1.70:9000/?command=getdevices
-- Result - {"30":{"deviceName":"Entry8","driverFileName":"light_v2.c4i","protocol":{"29":{"deviceName":"Entry8","driverFileName":"adaptive_phase_dimmer.c4i"}},"roomId":12,"roomName":"Lower"},"31":{"deviceName":"Stairs6","driverFileName":"adaptive_phase_dimmer.c4i","proxies":{"32":{"deviceName":"Stairs6","driverFileName":"light_v2.c4i"}}, ......}

How to use:
------------

Use the updated homekit plugin.   Otherwise, hit the endpoint http://10.0.1.70:9000/?command=getdevices to find the devices and filter by driverFileName.  Use  http://10.0.1.70:9000/?command=getvariables2&proxyID=25 to grab details about the variables for the devices.  Then you can get and set the variables to your hearts content.

- You need to either have access to ComposerPro or ask your dealer to install it
for you.
- Copy file web2way.c4i to C:\Program files\Control4\Composer2106\Drivers\virtual\
- On right side of composer, click search "Web 2-way"
- Drag into project
- Test port with  http://10.0.1.70:9000/?command=getdevices

Warning:
---------
This is completely unencrypted and unsecure. Anybody with access to your wifi
will be able to control any of your control4 connected devices. So don't open
it to internet and also make sure you have a secured wifi.
