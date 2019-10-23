

# 'Brilliant Smart WiFi Plug' with Homekit

The 'Brilliant Smart WiFi Plug' available from Bunnings or Officeworks in Australia is an ESP8266 / Tuya based smart plug. Thanks to the work of maximkulkin and gruppio, a full homekit switch implementation can be loaded on these if you are handy with electronics.

This project makes the **Bunnings 'Brilliant Smart WiFi Plug** compatible with **Apple Homekit**!
<img src="https://raw.githubusercontent.com/yahms/Homekit-BrilliantPlug/master/resize.jpg" alt="This"/>
<img src="https://raw.githubusercontent.com/Gruppio/Sonoff-Homekit/images/images/homekit.png" alt="Works with Apple Homekit" width="180"/>
 
As per Gruppio's project, this firmware doesn't require anything else running in order to work. It is not based on the Tasmota Fw, you don't need HomeBridge running on a Raspberry-Pi or an MQTT server, since HomeKit runs natively just like any other homekit device from the shops.

The implementation uses Apple provided specifications for device makers, so it should keep working for a long time.

This also means there is a cheap HomeKit smart plug available for Australian users (since in australia regulations require Live and Neutral switching - a normal SonOff only switches live). Should be idiot proof?

what you need:

* an apple iphone or ipad though you knew that already didnt you, right?
* wifi at home
* Smart plug
* 3.3v serial adapter
* wires
* Soldering iron


### Compatible Devices
This Software is currently tested on: 

Brillo Smart Wifi Plug with USB

---

## Other features

### Web Page Controller
For control your Sonoff from a non Apple device just navigate to the Plug's IP address and a web page will allow you to turn it on or off

### Rest APIs
A full set of Rest APIs are available:
* **/on**
* **/off**
* **/toggle**
* **/state**

All the request are in **GET** and are relative to the IP address of the plug
In order to turn on the plug at IP 192.168.0.22 you can: `$ curl 192.168.0.22/on`

### AutoReconnect after power outage
A problem with the old firmware was that after a power outage the Sonoff was immediately searching for the stored WIFI connection, but since the router was still powering on the Sonoff was prompting the configuration procedure. Now this problem is fixed, if the Sonoff does not have a WIFI Connection every 10min the Sonoff will restart.

### unSelectable PowerOn state
it defaults to on. let me know in issues if u need a default-off


### touch-free easy config wipe
in the absence of a button its hard to reset the wifi/homekit config, so you can go to "/resetconfig" and it will wipe the config including wifi details instantly

i figure you can always get it joining wifi coz if it cant find the configured wifi it should go to AP mode after a while

---

## Installation Instructions

### Flash the Sonoff
 1) pull apart the bunnings plug (remove the 2 screw covers, then pry open the plate.)
 2) remove the board with 2 more smaller screws
 3) solder 4 wires to the chip and connect wires to a serial adapter @ 3.3v
 4) boot up in bootloader mode by shorting IO0 to ground
 5) make sure no other serial devices are connected! and then run esptool.py erase_flash
 6) Run the `flash.sh` script 

### Add Plug to Home app
 1) Connect your iPhone or iPad to the new wifi network `Brillo-xxx`
 2) Wait for the Captive Portal and select your WiFi network
 3) Insert your WiFi Password
 4) Open the `Home` app
 5) Click the `+` symbol
 6) Click `I don't have the code...`
 7) Select the Brillo-xxx Switch 
 7.1 If the Brillo-xxx does not appear on top of the page wait a minute or kill the Home App
 9) Confirm that you want to add the plug
 10) Insert the Password that is `123-45-678`

Done! 


---

## bugs and issues

1) any reset or removal of wifi network will trigger a reboot, which will reset the relay state to the default. i would love to set it to just re-sttempt wifi connection rather than a full restart
2) the button definately doesnt work , likely because the Brilliant wires the button to GPIO14 and either the ESP or esp-rtos doesnt like interrupts on high gpio numbers ( i tested on a wemos d1 mini and an interrupt/button press on gpio2 worked fine )
3) the dodgy /resetconfig button workaround is definately a denial of service attack surface

---

#### Special thanks to:
@maximkulkin
@gruppio

This project would not have existed without:
https://github.com/maximkulkin/esp-homekit
https://github.com/maximkulkin/esp-homekit-demo
https://github.com/maximkulkin/esp-wifi-config
https://github.com/Gruppio/Sonoff-Homekit

