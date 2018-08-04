# raspi-troubleshooter
A step-by-step approach to troubleshooting a Raspberry Pi computer running Raspbian

[Skip ahead to troubleshooting section](#top)

> A credit card—sized single-board computer, the **Raspberry Pi** is affordable-yet-powerful with four cores of processing power at a cost of approximately US$35
> 
> **Raspbian** is the underlying operating system for Raspberry and is itself based on Debian

## Overview
I've created this interactive troubleshooter specifically to assist with a Raspberry computer which has been imaged with [OctoPi](https://github.com/guysoft/OctoPi); however, I've also included information about the native Raspbian-imaged computers as well. I assume that Raspbian Stretch is the underlying operating system regardless.

> **OctoPrint** is the leading web software for controlling 3D printers, created/maintained by Gina Häußge
>
> **OctoPi** is a Raspberry-specific distro of OctoPrint, maintained by Guy Sheffer

## <a name="top"></a>Start Here
Select a link throughout this guide to move to the next set of information and instructions.

* [Download OctoPrint for a Raspberry (OctoPi)](#head_Download)
* [Burn OctoPi image to microSD](#head_Image)
* [Prepare microSD for wi-fi connectivity](#head_PrepareForWifi)
* [Bring up the OctoPrint console in a browser](#head_OctoPrint)
* [OctoPrint's Setup Wizard](#head_SetupWizard)
* [Remote in with SSH](#head_RemoteSSH)
* [Enable SSH](#head_EnableSSH)
* [SSH won't allow connection](#head_EnableSSH)
* [Change the default hostname](#head_ChangingHostname)
* [New installation, no wi-fi connection](#head_NewInstallationNoWifiConnection)
* [Wi-fi can't see hidden zone](#head_WifiCannotSeeHiddenZone)
* [Scan for wi-fi zones](#head_ScanForWifi)
* [Lightning Bolt seen in Desktop](#head_LightningBolt)
* [Log in locally](#head_LoginLocally)
* [Edit files on the microSD's boot partition from a workstation](#head_Mount-microSD)
* [Connect an Ethernet cable](#head_Ethernet)
* [Check for throttling](#head_TestThrottling)
* [Find the Raspberry's IP address](#head_FindIP-Address)
* [Raspi won't connect to wi-fi](#head_WifiCannotConnect)
* [Test name resolution](#head_TestNameResolution)
* [Turn off Bluetooth](#head_TurnOffBluetooth)
* [Test wi-fi signal strength](#head_TestWifiStrength)
* [Interpret LED lights on Raspberry](#head_LED-Lights)
* [Return to start](#top)

## <a name="head_Download"></a>How to download OctoPi
Guy Sheffer maintains a Raspberry Pi—specific image of OctoPrint which includes support for a webcam. Having downloaded the image, you'll next want to burn the image to a microSD card which is at least 4GB in size although 8GB or greater would be recommended if you have a webcam.

* Download [OctoPi](https://github.com/guysoft/OctoPi) from the repository
* [Burn OctoPi image to microSD](#head_Image)
* [Return to start](#top)

## <a name="head_Image"></a>How to burn OctoPi to microSD
On OSX, either ApplePi-Baker or Etcher are good options for burning the previously-downloaded OctoPi image file to the microSD card. For Windows, Etcher is a personal favorite.

> **Etcher** is a graphical SD card writing tool that works on Mac OS, Linux and Windows, and is the easiest option for most users. Etcher also supports writing images directly from the zip file, without any unzipping required.

> **ApplePi-Baker** is for MacOS X only, allowing you to prepare an SD-Card for use with a Raspberry Pi. Its internal method of writing to the media appears to be faster than copying to the standard device itself with `dd`.

Place the microSD card into a USB or SD style of adapter and then insert it into your workstation. Running either Etcher or ApplePi-Baker, select the recently-downloaded image file (it's unnecessary to unzip it) and choose the option to burn it to the microSD card.

Since either software should auto-eject the microSD after burning, it would be necessary to remove/re-insert in order to do the minimal edits to enable wi-fi connectivity. (Attempt to safely eject the microSD first, though, if this is an option.)

* Download [Etcher](https://etcher.io)
* Download [ApplePi-Baker](https://www.tweaking4all.com/software/macosx-software/macosx-apple-pi-baker/)
* Prepare for wi-fi connectivity with [minimum edits](#head_PrepareForWifi)
* [Return to start](#top)

## <a name="head_PrepareForWifi"></a>Minimum edits required before using burned microSD
In order to automatically connect to your wi-fi network, some minimal edits to the files on the microSD card are necessary before attempting to boot up OctoPi. Answer the question below to be directed to the appropriate section:

* Is your wi-fi zone hidden? [yes](#head_WifiCannotSeeHiddenZone) [no](#head_WifiCannotConnect)
* [Return to start](#top)

## <a name="head_NewInstallationNoWifiConnection"></a>New installation, no wi-fi connection
Read below and follow a link if it applies to you:

* Is your wi-fi zone hidden? [yes](#head_WifiCannotSeeHiddenZone)
* Did you set the country code for your wi-fi zone? [no](#head_WifiCannotConnect)
* Wi-fi still won't connect? [yes](#head_WifiCannotConnect)
* [Return to start](#top)

## <a name="head_WifiCannotSeeHiddenZone"></a>Wi-fi cannot see hidden zone... or need to edit boot files
A hidden wi-fi zone means that it doesn't broadcast its name (SSID) every minute. As a result of this, your Raspberry's configuration must be done manually.

As an alternate choice, connect an Ethernet cable for connectivity.

It's necessary to add the `scan_ssid=1` command to one of two files, depending upon how you're setting this up:

* I setup my Raspberry with an [OctoPi image](#head_WifiCannotSeeHiddenZoneOctoPi)
* I setup my Raspberry with a [Raspbian image](#head_WifiCannotSeeHiddenZoneRaspbian)
* [Connect Ethernet cable](#head_Ethernet)
* [Return to start](#top)

## <a name="head_WifiCannotConnect"></a>Wi-fi won't connect... or need to edit boot files

* I setup my Raspberry with an [OctoPi image](#head_WifiCannotConnectOctoPi)
* I setup my Raspberry with a [Raspbian image](#head_WifiCannotConnectRaspbian)
* [Connect Ethernet cable](#head_Ethernet)
* [Return to start](#top)

## <a name="head_ScanForWifi"></a>Scan for wi-fi zones
Running the following from your Raspberry will indicate what wi-fi zones that it can see, the relative strength of each signal and the competition for those coveted channels: 1, 6 and 11.

```
sudo iwlist wlan0 scan | grep -v "IE:" | grep -v "Extra:" | grep -v "Mode:" | grep -v "Encryption key" | grep -v "Mb/s" | grep -v "Cipher" | grep -v "Authentication"
```

```
wlan0     Scan completed :
          Cell 01 - Address: 92:3B:AD:B1:E8:AD
                    Channel:4
                    Frequency:2.427 GHz (Channel 4)
                    Quality=70/70  Signal level=-34 dBm  
                    ESSID:"Nacho-Wi-Fi"
          Cell 02 - Address: 5C:8F:E0:59:37:ED
                    Channel:1
                    Frequency:2.412 GHz (Channel 1)
                    Quality=47/70  Signal level=-63 dBm  
                    ESSID:"5937ED"
          Cell 03 - Address: 10:0D:7F:DB:7E:AC
                    Channel:3
                    Frequency:2.422 GHz (Channel 3)
                    Quality=39/70  Signal level=-71 dBm  
                    ESSID:"DB7EAC"
          Cell 04 - Address: 96:3B:AD:B1:E8:AD
                    Channel:4
                    Frequency:2.427 GHz (Channel 4)
                    Quality=70/70  Signal level=-35 dBm  
                    ESSID:""
          Cell 05 - Address: 96:3B:AD:B3:D8:46
                    Channel:4
                    Frequency:2.427 GHz (Channel 4)
                    Quality=66/70  Signal level=-44 dBm  
                    ESSID:""
          Cell 06 - Address: 92:3B:AD:B3:D8:46
                    Channel:4
                    Frequency:2.427 GHz (Channel 4)
                    Quality=65/70  Signal level=-45 dBm  
                    ESSID:"Nacho-Wi-Fi"
          Cell 07 - Address: 38:70:0C:77:8A:56
                    Channel:6
                    Frequency:2.437 GHz (Channel 6)
                    Quality=41/70  Signal level=-69 dBm  
                    ESSID:"778A56"
          Cell 08 - Address: 88:D7:F6:65:0B:A8
                    Channel:10
                    Frequency:2.457 GHz (Channel 10)
                    Quality=57/70  Signal level=-53 dBm  
                    ESSID:"RT-AC1750_B1_A8_2G"
          Cell 09 - Address: FC:3F:DB:68:90:DE
                    Channel:6
                    Frequency:2.437 GHz (Channel 6)
                    Quality=23/70  Signal level=-87 dBm  
                    ESSID:"DIRECT-DD-HP ENVY 7640 series"
          Cell 10 - Address: 10:0D:7F:E2:28:36
                    Channel:11
                    Frequency:2.462 GHz (Channel 11)
                    Quality=32/70  Signal level=-78 dBm  
                    ESSID:"E22836"
          Cell 11 - Address: 94:C1:50:C1:EB:22
                    Channel:10
                    Frequency:2.457 GHz (Channel 10)
                    Quality=30/70  Signal level=-80 dBm  
                    ESSID:"ATT427"
          Cell 12 - Address: F8:CF:C5:FE:AA:66
                    Channel:11
                    Frequency:2.462 GHz (Channel 11)
                    Quality=27/70  Signal level=-83 dBm  
                    ESSID:"MOTO1534"
          Cell 13 - Address: 2C:30:33:E0:77:BC
                    Channel:4
                    Frequency:2.427 GHz (Channel 4)
                    Quality=32/70  Signal level=-78 dBm  
                    ESSID:"NETGEAR05"
          Cell 14 - Address: 40:B0:34:8F:E3:18
                    Channel:9
                    Frequency:2.452 GHz (Channel 9)
                    Quality=21/70  Signal level=-89 dBm  
                    ESSID:"DIRECT-17-HP ENVY 4520 series"
          Cell 15 - Address: 58:EF:68:A4:32:23
                    Channel:9
                    Frequency:2.452 GHz (Channel 9)
                    Quality=17/70  Signal level=-93 dBm  
                    ESSID:"# TEAM RIOS"
          Cell 16 - Address: 38:3B:C8:11:81:7E
                    Channel:7
                    Frequency:2.442 GHz (Channel 7)
                    Quality=30/70  Signal level=-80 dBm  
                    ESSID:"dantauKING"
          Cell 17 - Address: 38:70:0C:57:51:9E
                    Channel:11
                    Frequency:2.462 GHz (Channel 11)
                    Quality=24/70  Signal level=-86 dBm  
                    ESSID:"2.4GhzDigitalFellatio"
          Cell 18 - Address: FA:8F:CA:50:FF:DC
                    Channel:1
                    Frequency:2.412 GHz (Channel 1)
                    Quality=24/70  Signal level=-86 dBm  
                    ESSID:""
          Cell 19 - Address: 38:3B:C8:28:BB:82
                    Channel:8
                    Frequency:2.447 GHz (Channel 8)
                    Quality=24/70  Signal level=-86 dBm  
                    ESSID:"Not Today"
```

The higher the first number in the Quality ratio, the better the signal strength. The higher (in negative-number terms) of the Signal Level value, the better the signal strength. So `-34 dBm` is better than `-86 dBm`, in other words.

Consider moving your Raspberry Pi closer to the router, adjusting your wi-fi router so that it uses a less-competitive channel or upgrading your router.

* [Connect Ethernet cable](#head_Ethernet)
* [Return to start](#top)

## <a name="head_WifiCannotSeeHiddenZoneRaspbian"></a>Wi-fi cannot see hidden zone (Raspbian)

Logging in locally or by remoting into your Raspberry via SSH (temporarily connecting an Ethernet cable for connectivity), run `sudo nano /etc/wpa_supplicant/wpa_supplicant.conf` and manually configure the wi-fi settings.

The `scan_ssid=1` value removes the requirement to scan for your hidden zone. Make sure the two-letter country code is correct and the code is in uppercase. The line with the country code should be above the network paragraph.

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

## <a name="head_WifiCannotConnectRaspbian"></a>Wi-fi cannot connect (Raspbian)... or need to edit config files

Logging in locally or by remoting into your Raspberry via SSH (temporarily connecting an Ethernet cable for connectivity), run `sudo nano /etc/wpa_supplicant/wpa_supplicant.conf` and manually configure the wi-fi settings. Make sure the two-letter country code is correct and the code is uppercase. The line with the country code should be above the network paragraph.

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

## <a name="head_WifiCannotSeeHiddenZoneOctoPi"></a>Wi-fi cannot see hidden zone (OctoPi)... or need to edit boot files
Either logging in locally or mounting the microSD on your workstation, create and/or edit the file `octopi-wpa-supplicant.txt` on the `boot` partition. Note that the SSID and PSK values are case-sensitive.

The `scan_ssid=1` value removes the requirement to scan for your hidden zone. Make sure the two-letter country code is correct and the code is uppercase. The line with the country code should be above the network paragraph.

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

## <a name="head_WifiCannotConnectOctoPi"></a>Wi-fi cannot connect (OctoPi)... or need to edit boot files
Either logging in locally or mounting the microSD on your workstation, create and/or edit the file `octopi-wpa-supplicant.txt` on the `boot` partition. Note that the SSID and PSK values are case-sensitive. Make sure the two-letter country code is correct and the code is uppercase. The line with the country code should be above the network paragraph.

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

## <a name="head_OctoPrint"></a>Bring up the OctoPrint console in a browser
For OSX users (or Windows/Linux users who have installed Bonjour), in your workstation's browser visit the `http://octopi.local` URL.

For Windows users, it might be necessary to use the IP address of the Raspberry instead of its hostname by substituting it during the browser session which might look like `http://192.168.1.20`, for example.

* [Visit OctoPrint the first time after a new install](#head_SetupWizard)
* [Test name resolution](#head_TestNameResolution)
* [Find the Raspberry's IP address](#head_FindIP-Address)
* [Return to start](#top)

## <a name="head_SetupWizard"></a>OctoPrint Setup Wizard
When visiting the OctoPrint console the first time after a new install, a Setup Wizard will be presented.

During the setup, configuring **Access Control** is an option or may be left to the default. This would require you to log into OctoPrint when visiting its URL.

There is an option for configuring the default **printer profile**. It would be necessary to enter some details about your printer's dimensions and behavior.

Likewise, there is an option to enable/disable an **Internet connectivity check**. This would minimize processing resource-intensive operations like checking for updates if there's no connectivity.

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

## <a name="head_LED-Lights"></a>Understand LED lights on Raspberry
All Raspberry Pi boards have status LEDs positioned in one corner next to the 3.5mm audio jack and USB port(s). The number of LEDs is based upon which model.

1. `ACT` – D5 (Green) – SD Card Access. The normal status is to flash when the microSD is read from or written to. While Raspbian is attempting to first get booted, this will be a faint green glow. If it never goes to a brighter green then it was a boot failure. If the light flashes brightly from one to eight times and will not boot, the number is indicative to where it failed in the boot process.
2. `PWR` – D6 (Red) – 3.3 V Power is present. The normal status is to remain on. This LED will intermittently go off during moments when the input voltage falls below 4.64V DC. A blinking LED indicates a problem.
3. `FDX` – D7 (Green/Orange) – Full Duplex (LAN) connected. The normal status is on when Ethernet is in full duplex.
4. `LNK` – D8(Green/Orange) – Link/Activity (LAN). The normal status is on when Ethernet is linked to a hub, router or another adapter via a cross-over cable.
5. `100` – D9(Yellow/Orange) – 100Mbit (LAN) connected. The normal status is on when 100Mbps and off when only 10Mbps.

* [Adafruit blog post regarding LEDs](https://blog.adafruit.com/2013/02/15/raspberry-pi-status-leds-explained-piday-raspberrypi-raspberry_pi/)
* [Raspberry Pi troubleshooting](https://elinux.org/R-Pi_Troubleshooting#Normal_LED_status)
* [Return to start](#top)

|Description|Version|Author|Last Update|
|:---|:---|:---|:---|
|raspi-troubleshooter|v1.0.4|OutsourcedGuru|June 13, 2018|

|Donate||Cryptocurrency|
|:-----:|---|:--------:|
| ![eth-receive](https://user-images.githubusercontent.com/15971213/40564950-932d4d10-601f-11e8-90f0-459f8b32f01c.png) || ![btc-receive](https://user-images.githubusercontent.com/15971213/40564971-a2826002-601f-11e8-8d5e-eeb35ab53300.png) |
|Ethereum||Bitcoin|