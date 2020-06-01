**NOTE: This tutorial is geared towards low-end users who are just looking to run a node to help support the Decred network. For that reason, it does not dive into staking, setting up a wallet, or anything else beyond getting the node running with TOR.**

# Raspberry Pi Full Node With Tor Guide

In this tutorial, I’ll walk you through how to setup and run a Decred Full Node on the Raspberry Pi with Tor enabled. 

A full node is a program that fully validates transactions and blocks, without having to rely on a third party.

Running a full node is one of the strongest actions of support you can do for a peer-to-peer distributed protocol. Every single Decred node that runs on the network adds strength and resilience to the consensus mechanism.

While this process can be intimidating, it’s actually quite straightforward. I’d like to thank Decred community member’s Checkmate, Kozel, and Geostone for helping me out.

So lets start:

You’ll need:

- Raspberry pi 3b+ or higher
- Power Adapter (Power Cord)
- 32 Gig (or more) SD card and SD card reader, You can use an SSD instead of an SD card if you’d like to get the best performance and shelf life out of your node
- An HDMI cable if you’d like to hook your Pi directly up to a monitor or TV. 

If you don’t already have a Raspberry Pi, I’d recommend buying a kit or bundle which will come with everything you need. These usually run between $60 to $100 online, depending on what model you go for.

Start by plugging in your SD card or SSD into your computer. We'll need to install the Raspberry Pi OS. Download a program called the [Raspberry Pi Imager](https://www.raspberrypi.org/downloads/) and install it. Choose your operating system (default is fine), choose your SD card (be careful not to pick anything else) and click write.

You’ll need to decide if you want to set your pi up “headless” meaning you access your pi remotely without having to plug in a mouse, keyboard, and monitor, or you can choose to plug everything in and connect your pi to a tv or monitor with an HDMI cable.

If you want to do the headless approach, **you’ll need to create a text file in your boot drive titled** ```ssh```

If you plan on sshing into your pi wirelessly, there’s another step where you need to create a text file titled ```wpa_supplicant.conf``` and input the following in the document:

```
country=US
ctrl_interface=DIR=/var/run/wpa_supplicant  GROUP=netdev
update_config=1

network={
  scan_ssid=1
  ssid="your_wifi_ssid"
  psk="your_wifi_password"
  key_mgmt=WPA-PSK
}
```
Of course you need to input your own country, ssid (Network name), and your own network password.

Save the file once you are done.

Right click the drive and click eject.

Plug your SD card into your Pi, plug in the power, and boot up your pi.


If you are doing this process directly connecting your pi to a monitor, you should see the pi boot up and you’ll be presented with the homescreen.

If you are doing a headless setup you will need to download a program called [PUTTY](https://putty.org/). This program will allow us to access our pi, and control it via command line after it has booted.

In PUTTY, enter ```raspberrypi.local``` and click enter. Click connect past the precaution.
You’ll be prompted to enter a Username and Password. The default is ``` pi```  and the password is ``` raspberry```  (all lowercase).

Congrats, you have just SSHed into your pi.

If you tried to connect wirelessly and it couldn't find a host, you may need to plug in your pi directly to a monitor and manually enter your network passphrase when prompted. You can also just go directly through ethernet, which is the easier option.

Now we need to enable VNC.

In the command line type

```sudo raspi-config```

Select **Interfacing Options** 

Select **VNC**

Press yes, then okay, then finished.

Next we'll set the resolution for VNC. Run:

```sudo nano /boot/config.txt```

Add the following text to the document:
```
framebuffer_width=1900
framebuffer_height=1024
```



Now download, install and launch [VNC Viewer](https://www.realvnc.com/en/connect/download/viewer/)

Select New connection from the File menu

Enter ```raspberrypi.local``` in the "VNC Server" field

Click Ok.

Double-click on the connection icon to connect.

Click Ok if you are shown a security warning.

Enter the Pi's username and password when prompted. The defaults are username: ``` pi```  and password: ``` raspberry```  Click Ok.

Your Raspberry Pi desktop will then appear in a window on your main computer's desktop. You'll be able to control everything from there.

Click past the SSH warning.

Set your country as needed.

Change your Pi account’s password when prompted

You can configure your wi-fi at this point, if needed.

Update your Pi.

Reboot your Pi when prompted.

If you’re doing this over VNC you’ll need to reconnect once the pi has re-booted.

Now we need to download the DCR installer.

Open the Web Browser.

Navigate to the decred.org website.

Scroll down and look for the green "Stable V1.5.1" button.

Clicking this will bring us to Decred's github releases page.

Download ```dcrinstall-linux-arm-v1.5.1```

There may be a newer version out at the time of this video. Make sure to download the latest version.

We'll need to set the file as an exectuable. Open the Pi Terminal.

Run ```sudo chmod u+x dcrinstall-linux-arm-v1.5.1```

Navigate to your downloads folder. Double click the installer, execute the installer in Terminal

It will take a few minutes to download and setup all the files. A folder called ./decred has now been placed in your home directory. 

It may ask you to input a password for a new wallet. We won’t be using this as a wallet so you can go through quickly.

Type n for public data

Hit n for Seed

Hit okay for the seed.


Wait for it to finish.


Now we can start the node:

Open the terminal.

Run:

Enter 
```
cd ./decred/decred-linux-arm-v1.5.1
./dcrd
```

Make sure to change the version number if the current version is no longer v1.5.1 (e.g v1.6.0)

The Decred daemon will boot up and start connecting to peers

If you see that your node has successfully booted, hit control+c so we can finish by enabling TOR. 

TOR is free and open-source software for enabling anonymous communication.

Open the raspberry pi terminal:

Run ```sudo apt install tor```

Press y to continue

Run  ```sudo nano /etc/tor/torrc```

Add the following text at the top:
```
SocksPort 9050
SocksPort raspberrypi.local:9050
RunAsDaemon 1
DataDirectory /var/lib/tor
HiddenServiceDir /var/lib/tor/dcrd
HiddenServiceVersion 2
HiddenServicePort 9108 127.0.0.1:9108
```

Press Control and X at the same time when you are finished. Save to the same file location.
Hit enter to continue.

restart the tor service with ```sudo systemctl restart tor@default.service```

Check Status to see if its working ```sudo systemctl status tor@default.service```

(Exit view with ctrl+C)

Run ```sudo cat /var/lib/tor/dcrd/hostname```

Save your .onion **we will need it for the next step.**

Run ```nano .dcrd/dcrd.conf```

Edit the top of the file with:
```
proxy=127.0.0.1:9050
listen=
externalip=Your-Onion-From-Above.onion
torisolation=1
```
Press Control+X to exit.

Save file

Run your Decred node with

```
cd
cd ./decred/decred-linux-arm-v1.5.1
./dcrd
```
The Node will need lots of time to download and sync. Currently the Decred Blockchain is 4.2 Gigabytes in size.


When you start seeing logs saying **(inbound)** it means your node is accepting peer connections and now you’re officially part of the Decred network, helping it grow. 

If your Pi ever loses power, make sure to reboot it and re-run dcrd. It's also a good idea to use VNC to check up on your Pi every so often. Also make sure you follow the Decred Project so you don't miss any new releases.
