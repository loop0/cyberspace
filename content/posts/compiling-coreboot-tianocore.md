---
title: "Compiling coreboot + tianocore for my thinkpad x220"
date: 2020-05-24
tags: ["coreboot", "tianocore", "thinkpad", "x220"]
draft: false
---

Recently I flashed coreboot + tianocore into my thinkpad x220 and I was having some issues to compile it on my Fedora 32. So I used docker to run an older version of the ubuntu to make it compile flawlessly.

Here is what I had to do, so inside my coreboot directory I ran the command:

```sh
docker run -it -v /home/loop0/Code/coreboot-4.12:/coreboot:z ubuntu:18.04 /bin/bash
```

Once inside bash I had to install a few packages:

```sh
apt-get update
apt-get install gnat build-essential libssl-dev pkg-config git flex bison libncurses5-dev wget zlib1g-dev uuid-dev nasm python
```

Then to proceed on the compilation:

```sh
cd /coreboot
make crossgcc-i386 CPUS=8
make -j8
```

At the end of the process if everything goes well you should have a file called `coreboot.rom` inside the `build` folder. That's it, you're ready to flash the new bios and make your laptop a little more free.