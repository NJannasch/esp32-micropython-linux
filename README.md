# esp32-micropython-linux
Helpful tips for using an esp32 together with linux

# ESP 32 Setup
## Firmware download
Download firmware: https://micropython.org/download#esp32  

## Get identifier of your ESP32
`dmesg | grep -i tty`  
[ 1327.980878] usb 2-1.4.4.2: cp210x converter now attached to ttyUSB6

Thereby we can see ttyUSB6 is used.
## Flashing
Install esptool:
`pip3 install esptool`

Erase the flash:  
`esptool --port /dev/ttyUSB6 erase_flash`

### ESP32
Deploy new firmware:  
`esptool --chip esp32 --port /dev/ttyUSB6 write_flash -z 0x1000 FIRMWARE.bin`

### ESP8266  
Deploy new firmware:  
`esptool --port /dev/ttyUSB0 write_flash --flash_size=detect 0 FIRMWARE.bin`

## Get connected
Install picocom:  
`sudo apt install picocom`  

Add current user into dialout group to access serial devices:  
`sudo adduser USERNAME dialout` (restart needed)

Connect to ESP32:  
`picocom /dev/ttyUSB6 -b115200`

Exit connection:  
`Ctrl-a`, then `Ctrl-q`

## Add device to network
```
import network

wlan = network.WLAN(network.STA_IF) # create station interface
wlan.active(True)       # activate the interface
wlan.scan()             # scan for access points
wlan.isconnected()      # check if the station is connected to an AP
wlan.connect('essid', 'password') # connect to an AP
wlan.config('mac')      # get the interface's MAC adddress
wlan.ifconfig()         # get the interface's IP/netmask/gw/DNS addresses
```

## Control output pin
```
import utime
import machine

pin13 = machine.Pin(13, machine.Pin.OUT)
while True:
    pin13.value(1)
    utime.sleep_ms(500)
    pin13.value(0)
    utime.sleep_ms(500)
```

## Python libs
Available python libs:  
https://github.com/micropython/micropython-lib

## Install libs
Install upip on ESP32 (download and copy to `lib/`)  
`import upip`  
`upip.install('micropython-uasyncio', 'lib')`

## Useful tools
PyCharm micropython plugin  
https://blog.jetbrains.com/pycharm/2018/01/micropython-plugin-for-pycharm/


### References:
* http://docs.micropython.org/en/latest/esp32/tutorial/intro.html
* https://www.cnx-software.com/2017/10/16/esp32-micropython-tutorials/
* http://docs.micropython.org/en/latest/esp8266/tutorial/repl.html
