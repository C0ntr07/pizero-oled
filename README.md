# pizero-oled
Step-by-step instructions for setting up a Raspberry Pi Zero W with an Adafruit 1.3" OLED bonnet

## Parts
For this, you'll minimally need:

* [Raspberry Pi Zero WH (a Zero W with header)](http://adafrui.it/3708) $14
* [Adafruit 1.3" OLED bonnet](http://adafrui.it/3531) $22.50
* A 4GB or bigger microSD adapter ~$7
* A standard Raspberry 2.6A 5V power supply with micro USB plug

## Instructions
Install the OLED hat on top of the headers provided with the Raspberry Pi Zero WH. Remove the film on the top by pulling up on the provided blue tab.

1. Download [Raspbian Stretch Lite](https://www.raspberrypi.org/downloads/raspbian/) and using something like [Etcher](https://etcher.io), burn that image to the microSD adapter while in your workstation
2. Once finished, Etcher will automatically eject the media. Remove and replace the microSD to re-mount it.
3. In a Terminal, run the following commands (assuming OSX):
4. `sudo touch /Volumes/boot/ssh`
5. `sudo touch /Volumes/boot/wpa_supplicant.conf`
5. `sudo nano /Volumes/boot/wpa_supplicant.conf`

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US
 
network={
        ssid="insert_your_hidden_SSID_here"
        psk="insert_your_wifi_password_here"
        key_mgmt=WPA-PSK
}
```

Save the file, safely eject the media and put it into the Raspberry Pi.

### Remote into the Raspberry

1. `ssh pi@raspberrypi.local` and use `raspberry` as the default password
2. `sudo apt-get update`
3. `sudo apt-get upgrade -y`
4. `sudo raspi-config`
	1. 7 Advanced Options -> A1 Expand Filesystem
	2. 2 Network Options -> N1 Hostname (I'm changing mine to `glacier`)
	3. 3 Boot Options -> B1 Desktop/CLI -> B2 Console Autologin
	4. 4 Localisation Options -> I1 Change Locale, I2 Change Timezone, I3 Change Keyboard Layout and I4 Change Wi-Fi Country
	5. 5 Interfacing Options -> P5 I2C
	6. 8 Update
	7. Finish and reboot (use `sudo reboot` if it doesn't do it for you)
5. `ssh pi@glacier.local` (or whatever you named yours)
6. `sudo apt-get install build-essential python-dev python-pip -y`
7. `sudo pip install RPi.GPIO` # Possibly already installed
8. `sudo apt-get install python-imaging python-smbus i2c-tools -y`
9. `sudo apt-get install git -y`
10. `mkdir ~/sites && cd ~/sites`
11. `git clone https://github.com/adafruit/Adafruit_Python_SSD1306.git`
12. `cd Adafruit_Python_SSD1306`
13. `sudo python setup.py install`
14. `sudo i2cdetect -y 1`

```
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- -- 
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
30: -- -- -- -- -- -- -- -- -- -- -- -- 3c -- -- -- 
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 
70: -- -- -- -- -- -- -- -- 
```

This means that the OLED was found at address `0x3c`.

&nbsp;15. `sudo python examples/buttons.py`

Press each button and exercise the joystick to see all the interactions. Press Ctl-C to exit from this Python script.

Also try `sudo python examples/animate.py`, `sudo python examples/image.py`, 'sudo python examples/shapes.py' and `sudo python examples/stats.py`. You may want to edit each of these by commenting out this line:

```
disp = Adafruit_SSD1306.SSD1306_128_32(rst=RST)
```

...and uncommenting this one:

```
disp = Adafruit_SSD1306.SSD1306_128_64(rst=RST)
```

...which is the appropriate driver for this display (64 pixels tall).

&nbsp;16. `sudo nano /boot/config.txt`

Add this to the bottom of the file:

```
# Speed up the display to 1Mhz
dtparam=i2c_baudrate=1000000
```

&nbsp;17. `sudo reboot`

|Description|Version|Author|Last Update|
|:---|:---|:---|:---|
|pizero-oled|v1.0.1|OutsourcedGuru|July 7, 2018|

|Donate||Cryptocurrency|
|:-----:|---|:--------:|
| ![eth-receive](https://user-images.githubusercontent.com/15971213/40564950-932d4d10-601f-11e8-90f0-459f8b32f01c.png) || ![btc-receive](https://user-images.githubusercontent.com/15971213/40564971-a2826002-601f-11e8-8d5e-eeb35ab53300.png) |
|Ethereum||Bitcoin|