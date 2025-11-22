---
title: "BIOS update on Thinkpad 600E"
date: 2020-06-25
tags: ["thinkpad", "600E"]
draft: false
---

Plugged my IBM usb floppy drive into the 600E and ran the `spsdin36.exe E:` to write the files into the diskette.

Next step was to make an image from this diskette and make it a bootable CD so I could boot into my 600E.

Plugged the usb floppy into my main desktop running linux and did a couple of commands to make an image from the floppy and convert it into an iso cd image.

Making the floppy image:
```
sudo dd if=/dev/sdb of=spsdin36.img
```

Convert from floppy image to iso:
```sh
mkisofs -pad -b spsdin36.img -R -o spsdin36.iso spsdin36.img 
```

I burned the iso image to a cd and booted into my Thinkpad 600E with no problems.
The update process was like 20 seconds and then it asked me to reboot. Profit!