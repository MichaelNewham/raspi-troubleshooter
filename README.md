# raspi-troubleshooter
A step-by-step approach to troubleshooting a Raspberry Pi computer running Raspbian

## Overview
I've created this interactive troubleshooter specifically to assist with a Raspberry computer which has been imaged with [OctoPi](https://github.com/guysoft/OctoPi); however, I've also included information about the native Raspbian-imaged computers as well. I assume that Raspbian Stretch is the underlying operating system regardless.

## <a name="top"></a>Start Here
Select a link throughout this guide to move to the next set of information and instructions.

* [Need to remote in with SSH](#head_RemoteSSH)
* [Need to enable SSH](#head_EnableSSH)
* [SSH won't allow connection](#head_EnableSSH)
* [Need to change the default hostname](#head_ChangingHostname)
* [New installation, no wi-fi connection](#head_NewInstallationNoWifiConnection)
* [Wi-fi can't see hidden zone](#head_WifiCannotSeeHiddenZone)
* [Lightning Bolt seen in Desktop](#head_LightningBolt)
* [Need to log in locally](#head_LoginLocally)
* [Need to edit files on the microSD's boot partition from a workstation](#head_Mount-microSD)
* [Need to connect an Ethernet cable](#head_Ethernet)
* [Need to check for throttling](#head_TestThrottling)
* [Need to find its IP address](#head_FindIP-Address)
* [Raspi won't connect to wi-fi](#head_WifiCannotConnect)
* [Need to test name resolution](#head_TestNameResolution)
* [Need to turn off Bluetooth](#head_TurnOffBluetooth)
* [Need to test wi-fi signal strength](#head_TestWifiStrength)
* [Return to start](#top)

## <a name="head_NewInstallationNoWifiConnection"></a>New installation, no wi-fi connection
* Is your wi-fi zone hidden? [yes](#head_WifiCannotSeeHiddenZone)
* Did you set the country code for your wi-fi zone? [no](#head_WifiCannotConnect)
* Wi-fi still won't connect? [yes](#head_WifiCannotConnect)
* [Return to start](#top)

## <a name="head_WifiCannotSeeHiddenZone"></a>Wi-fi cannot see hidden zone
A hidden wi-fi zone means that it doesn't broadcast its name (SSID) every minute. As a result of this, your Raspberry's configuration must be done manually.

As an alternate choice, connect an Ethernet cable for connectivity.

It's necessary to add the `scan_ssid=1` command to one of two files, depending upon how you're setting this up:

* [OctoPi image](#head_WifiCannotSeeHiddenZoneOctoPi)
* [Raspbian image](#head_WifiCannotSeeHiddenZoneRaspbian)
* [Connect Ethernet cable](#head_Ethernet)
* [Return to start](#top)

## <a name="head_WifiCannotConnect"></a>Wi-fi cannot connect

As an alternate choice, connect an Ethernet cable for connectivity.

It might be necessary to edit a configuration file, depending upon how you're setting this up:

* [OctoPi image](#head_WifiCannotConnectOctoPi)
* [Raspbian image](#head_WifiCannotConnectRaspbian)
* [Connect Ethernet cable](#head_Ethernet)
* [Return to start](#top)

## <a name="head_WifiCannotSeeHiddenZoneRaspbian"></a>Wi-fi cannot see hidden zone (Raspbian)

Logging in locally or by remoting into your Raspberry via SSH (temporarily connecting an Ethernet cable for connectivity), run `sudo nano /etc/wpa_supplicant/wpa_supplicant.conf` and manually configure the wi-fi settings.

The `scan_ssid=1` value removes the requirement to scan for your hidden zone. Make sure the two-letter country code is correct.

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US
 
network={
        scan_ssid=1
        ssid="insert_your_hidden_SSID_here"
        psk="insert_your_wifi_password_here"
        key_mgmt=WPA-PSK
}
```

* [Log in locally to the Raspberry](#head_LoginLocally)
* [Remote in via SSH](#head_RemoteSSH)
* [Return to start](#top)

## <a name="head_WifiCannotConnectRaspbian"></a>Wi-fi cannot connect (Raspbian)

Logging in locally or by remoting into your Raspberry via SSH (temporarily connecting an Ethernet cable for connectivity), run `sudo nano /etc/wpa_supplicant/wpa_supplicant.conf` and manually configure the wi-fi settings. Make sure the two-letter country code is correct.

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

* [Log in locally to the Raspberry](#head_LoginLocally)
* [Remote in via SSH](#head_RemoteSSH)
* [Return to start](#top)

## <a name="head_WifiCannotSeeHiddenZoneOctoPi"></a>Wi-fi cannot see hidden zone (OctoPi)
Either logging in locally or mounting the microSD on your workstation, create and/or edit the file `octopi-wpa-supplicant.txt` on the `boot` partition. Note that the SSID and PSK values are case-sensitive.

The `scan_ssid=1` value removes the requirement to scan for your hidden zone. Make sure the two-letter country code is correct.

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US
 
network={
        scan_ssid=1
        ssid="insert_your_hidden_SSID_here"
        psk="insert_your_wifi_password_here"
        key_mgmt=WPA-PSK
}
```

* [Log in locally to the Raspberry](#head_LoginLocally)
* [Mounting the microSD in your workstation](#head_Mount-microSD)
* [Return to start](#top)

## <a name="head_WifiCannotConnectOctoPi"></a>Wi-fi cannot connect (OctoPi)
Either logging in locally or mounting the microSD on your workstation, create and/or edit the file `octopi-wpa-supplicant.txt` on the `boot` partition. Note that the SSID and PSK values are case-sensitive. Make sure the two-letter country code is correct.

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

* [Log in locally to the Raspberry](#head_LoginLocally)
* [Mounting the microSD in your workstation](#head_Mount-microSD)
* [Return to start](#top)

## <a name="head_LightningBolt"></a>Lightning Bolt
In the Desktop (PIXEL) version of Raspbian, a lightning bolt symbol will appear in the upper corner intermittently when the input voltage falls below the 4.65V DC threshhold.

* [Official specifications for Raspberry Pi 3](https://www.raspberrypi.org/documentation/hardware/raspberrypi/power/README.md)
* [Checking for throttling](#head_TestThrottling)
* [Return to start](#top)

## <a name="head_ChangingHostname"></a>Changing the hostname
The easiest way to change the hostname of a Raspberry Pi computer is by running the `sudo raspi-config` program after remoting into it and choosing option `2 Hostname`.

Expect a forced reboot after finishing, remembering to use the new hostname when reconnecting.

* [Return to start](#top)

## <a name="head_RemoteSSH"></a>Remote in with SSH
In Linux or OSX, the **ssh** command is used to remote into the Raspberry Pi. In Windows, the **putty** command is used (requires installation).

`ssh pi@raspberrypi.local` using the default "raspberry" password

If you have changed the hostname, then substitute this for "raspberrypi" in the command above. For an OctoPi-imaged Raspberry, the command would be `ssh pi@octopi.local`.

For Windows users, it might be necessary to use the IP address of the Raspberry instead of its hostname by substituting it during the putty session.

* [putty download](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) for Windows
* [Return to start](#top)

## <a name="head_EnableSSH"></a>Enable SSH
The **ssh** command is used in Linux/OSX for remoting into the Raspberry Pi's console; however, it is sometimes turned off by default, say, in the newest version of Raspbian. Do one of the following to enable SSH upon startup of the Raspberry:

1. Log in locally to the Raspberry and run `sudo raspi-config`, select `5 Interfacing Options` then `P2 SSH` to turn it on.
2. Mounting the microSD on your workstation, create an empty file called `ssh` in the `boot` partition. Safely **eject** the microSD, put it back into the Raspberry and boot it up.

For Windows users, it might be necessary to use the IP address of the Raspberry instead of its hostname by substituting it during the putty session.

* [Official documentation](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md)
* [Log in locally to the Raspberry](#head_LoginLocally)
* [Mounting the microSD in your workstation](#head_Mount-microSD)
* [Return to start](#top)

## <a name="head_LoginLocally"></a>Log in locally
Connect a monitor via the HDMI connection plus a keyboard and mouse to the Raspberry. Connect a 2.5A 5V micro-USB power adapter and boot it up.

At the prompt, enter `pi` as the username and `raspberry` as the default password. 

* [Return to start](#top)

## <a name="head_Mount-microSD"></a>Mount the microSD card in your workstation
Using an SD or USB adapter, insert the microSD card into your workstation. You should have a newly-mounted `boot` folder available to you.

Use a text editor for creating and/or editing any files. If in Windows, make sure to use Notepad++ instead of the default text editor.

Make sure to exit any editors and to safely eject the `boot` drive after you're finished.

* [Return to start](#top)

## <a name="head_FindIP-Address"></a>Finding the IP address of the Raspberry
Assuming that the Raspberry is either connected with an Ethernet cable or to a wi-fi zone, try one of the following options:

Log into your network router to determine the IP address and connectivity status of your Raspberry.

Logging in locally to the Raspberry or remoting in via SSH, run `ifconfig` to determine if the particular network adapter is up and bound to an IPv4 address.

From your workstation, try using the **ping** command to determine the Raspberry's IP address.

Use Adafruit's Raspberry Pi Finder software from your workstation to scan your local network.

* [Log in locally to the Raspberry](#head_LoginLocally)
* [Remote in via SSH](#head_RemoteSSH)
* [Using the ping command for name resolution](#head_TestNameResolution)
* [Adafruit Raspberry Pi Finder](https://learn.adafruit.com/the-adafruit-raspberry-pi-finder/overview) information
* [Return to start](#top)

## <a name="head_Ethernet"></a>Connecting to Ethernet
There are times when it's just easiest to connect the Raspberry using an Ethernet cable to your router or hub.

In the case of the Raspberry Pi 3 and similar, an RJ-45 connector is built into the computer.

If there isn't an RJ-45 connection, like on a Raspberry Pi Zero or Raspberry Pi Zero W computer, it would be necessary to first connect a USB-based Ethernet adapter for this type of connectivity.

* [Return to start](#top)

## <a name="head_TestThrottling"></a>Checking for throttling
Logging in locally or remoting in via SSH, run `vcgencmd get_throttled`. A non-zero value indicates that the Raspberry has throttled back the processor speed due to either under-voltage or an overheating condition.

Running `vcgencmd measure_temp` will show the temperature of the CPU in Celsius, which could then rule out temperature as the cause.

The minimum threshhold for power is 4.64V DC. Anything lower will trigger throttling. The default temperature limit for the CPU is 85 degrees before triggering throttling.

* [Log in locally to the Raspberry](#head_LoginLocally)
* [Remote in via SSH](#head_RemoteSSH)
* [Return to start](#top)

## <a name="head_TestNameResolution"></a>Testing name resolution
From your workstation, run `ping raspberrypi.local` or `ping octopi.local`, depending upon the Raspberry's hostname. You might need to press `Ctl-C` to cancel the ping command. You should see something like this:

```
PING octopi.local (192.168.1.25) 56(84) bytes of data.
64 bytes from 192.168.1.25: icmp_seq=1 ttl=64 time=14.6 ms
...
```

This would represent a successful name resolution lookup from `octopi.local` to its underlying IP address.

* [Return to start](#top)

## <a name="head_TurnOffBluetooth"></a>Turning off Bluetooth
The Bluetooth capabilities built into the Raspberry use the better of the two UARTs inside the main chip. By turning off the Bluetooth device driver, this can sometimes free up that good UART (like for a Prusa soldered installation of a Pi Zero).

Logging in locally or remoting in via SSH, run `sudo nano /boot/config.txt` and add at the bottom:

```
dtoverlay=pi3-disable-bt
```

Save and exit from **nano**. Follow-up with:

```
sudo systemctl disable hciuart.service
sudo systemctl disable bluealsa.service
sudo systemctl disable bluetooth.service
```

* [Return to start](#top)

## <a name="head_TestWifiStrength"></a>Testing wi-fi signal strength
Sometimes it's necessary to audit the wi-fi space to test signal strength and for the existence of competing neighbors' zones.

Logging in locally or remoting in via SSH, run the following:

```
sudo iwlist wlan0 scan | egrep "Cell|ESSID|Signal|Rates"
sudo iwconfig wlan0
```

* [Return to start](#top)


|Description|Version|Author|Last Update|
|:---|:---|:---|:---|
|raspi-troubleshooter|v1.0.3|OutsourcedGuru|June 13, 2018|

|Donate||Cryptocurrency|
|:-----:|---|:--------:|
| ![eth-receive](https://user-images.githubusercontent.com/15971213/40564950-932d4d10-601f-11e8-90f0-459f8b32f01c.png) || ![btc-receive](https://user-images.githubusercontent.com/15971213/40564971-a2826002-601f-11e8-8d5e-eeb35ab53300.png) |
|Ethereum||Bitcoin|