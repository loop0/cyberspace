---
title: "Realtek 8125 2.5Gb Ethernet work on FreeBSD"
date: 2025-06-01T20:36:10-04:00
draft: false
tags: ["freebsd"]
---

I decided to migrate my virtualization host to FreeBSD bhyve.
During installation in my current hardware and one of the issues I had was the
ethernet interface not being recognized in the system. Luckily I had access
through a usb ethernet so I could run the following steps to make my onboard
network work.

1. Install the package `realtek-re-kmod`:

```sh
pkg install realtek-re-kmod
```

2. Add the following 2 lines to your `/boot/loader.conf`:

```sh
if_re_load="YES"
if_re_name="/boot/modules/if_re.ko"
```

3. Reboot your server
4. You should be able to see the your device listed with `ifconfig`

```sh
re0: flags=1008943<UP,BROADCAST,RUNNING,PROMISC,SIMPLEX,MULTICAST,LOWER_UP> metric 0 mtu 1500
```

5. Cheers!