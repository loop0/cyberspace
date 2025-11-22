---
title: "Turning a Wired Keyboard Into Wireless"
date: 2024-01-17T20:20:26-05:00
draft: false
---

I currently type on a nice mechanical keyboard from MNT, the [MNT Reform Keyboard](https://shop.mntre.com/products/mnt-reform-usb-keyboard-standalone) but I wish it was wireless to have less cables running through my work desk.

So I came up with this project to try to turn this keyboard into a wireless one. The PoC I'm making is using 2x ESP32-S3
dev kit boards. The boards have 2 USB-C connections, one for programming and power and the other one can be used as USB-OTG. My idea is to have one board hooked up to a battery and to the keyboard and the other board hooked to the PC. Having the board receiving the keystrokes and sending them via wireless to the PC over a protocol called ESP-NOW.

I did some initial tests to verify I am able to have the keyboard side working as expected. So let's go into details
on what I've done so far:

**The boards**
The board I'm using is a `Teyleten Robot ESP32-S3-DevKitC-1-N8R2` which is basically a clone of the oficial one from Espressif but with USB-C ports instead of Micro USB

**Programming**
To do the board programming I followed the [setup procedure](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/get-started/linux-macos-setup.html) from Espressif documentation, but TL;DR is:

```sh
brew install cmake ninja dfu-util
mkdir -p ~/esp
cd ~/esp
git clone --recursive https://github.com/espressif/esp-idf.git
cd esp-idf
./install.fish esp32s3
. $HOME/esp/esp-idf/export.fish
```

By this point you should have pretty much all you need to compile and flash the board.

**Testing***
To test the USB host capability of the board I used the example provided and ran the following commands:

```sh
cd ~/esp/esp-idf/examples/peripherals/usb/host/hid
idf.py set-target esp32s3
idf.py build
idf.py flash
```

This will flash the host HID example into the board, but before we can plug the keyboard we need to solder a jumper for the USB-OTG on the back of the board like this:

![ESP32-S3 Board USB-OTG Jumper](/esp32-jumper.jpg)

With that soldered I was able to verify that the board can host the keyboard and power it through the USB-OTG port!

![ESP32-S3 USB Keyboard](/esp32-keyboard.jpg)

And I was also able to verify the board receiving the keypresses by monitoring the serial connection with the following command:

```sh
idf.py monitor
```

Hosting a USB HID device is what I believe to be the hardest part on this PoC, and with this initial verification that it works I'm pretty confident in continue to work on this. So the following steps on this project are:

* Connect the two boards using ESP-NOW
* Write code for the board that will be connected to the PC to act as a keyboard device
* Write the code to send the keypresses from the keyboard to the PC using the ESP-NOW connection
* Verify that I can type on the computer wirelessly through the PoC solution

The end goal is that after I can get this PoC working I want to design a board for the keyboard side that will integrate the ESP32 module, a battery, a recharging circuit and a 3d printed case to fit on the keyboard. Everything will be documented here
 and the code and designs will be opensourced.

Come hangout and let me know your thoughts on mastodon at [@loop0@freeradical.zone](https://freeradical.zone/@loop0)




