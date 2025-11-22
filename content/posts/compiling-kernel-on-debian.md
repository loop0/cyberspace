---
title: "Compiling Kernel on Debian With Secure Boot Support"
date: 2025-11-21T09:40:58-05:00
draft: false
tags: ["debian", "linux"]
---

# Compiling a custom kernel on debian with support for secure boot

I recently installed debian 13 on my workstation and I wanted to have a newer kernel, and also experiment with PREEMPT_RT.
So I decided to learn how to compile a custom kernel and I will try to document the steps here for future reference.

## Steps

> `INSTRUCTIONS`:
> The character `$` found at the beginning tells that the following is a command to be run in a shell.
> If you are copy and pasting, make sure you ommit the `$` char.

This was executed on stock debian 13.2 YMMV:

1. Download the kernel source code tarball from [https://kernel.org](https://kernel.org)
2. Install the following dependencies:
```sh
$ sudo apt install build-essential dkms flex bison libdw-dev libelf-dev libssl-dev libssl-dev rsync debhelper-compat sbsigntool ncurses-dev
```
3. Generate the dkms signing keys
```sh
$ sudo dkms generate_mok
```
4. Import your dkms keys to be enrolled in secure boot
```sh
$ sudo mokutil --import /var/lib/dkms/mok.pub
```
5. Reboot your computer, your bios should show a utility to enroll the newly imported key
6. Lets export certificate of the key you just enrolled
```sh
$ cd /tmp
$ sudo mokutil --export
# This can generate many files with the filename following the pattern MOK-0001.der
# Run the following command file by file until you find your DKMS keys
$ openssl x509 -inform DER -in MOK-0002.der -text -noout | grep "Subject:"
    Subject: CN=DKMS module signing key
# Once you find the key with the DKMS subject you can export the certificate
$ sudo openssl x509 -inform DER -in MOK-0002.der -outform PEM -out /var/lib/dkms/mok.crt
```
7. Lets proceed to extract the linux kernel source code from the tarball you download on step 1
```sh
$ tar xfv ~/Downloads/linux-6.17.8.tar.xz
$ cd linux-6.17.8
```
8. Now is the time to configure your kernel, we begin by using the config provided by the current kernel as a starting point
```sh
$ cp /boot/config-6.12.57+deb13-amd64 .config
```
9. Lets disable the debug info (optional)
```sh
$ scripts/config --disable DEBUG_INFO_DWARF_TOOLCHAIN_DEFAULT
$ scripts/config --disable DEBUG_INFO
```
10. One last step before compiling the kernel, lets copy the signing key
```sh
$ sudo cp /var/lib/dkms/mok.key certs/mok.key
$ sudo cat /var/lib/dkms/mok.crt >> certs/mok.key
```
11. Now to the step that we compile the kernel and generate a .deb package for installation
```sh
# Adjust the number from -j16 to the number of cores your computer has for faster compilation
# LOCALVERSION is optional, you can change it to anything you want, it will become part of the kernel version
# The make command will ask you for configuration options that don't exist from the current kernel config that
# we used as a starting point, you can proceed to just hit return to use the default or run make nconfig to 
# configure your kernel using a terminal UI
$ make -j16 bindeb-pkg CONFIG_MODULE_SIG_KEY=certs/mok.key SIGN_KEY=certs/mok.key LOCALVERSION=-loop0-rt
```
12. Before you install the newly generated debian package lets add a kernel post install hook to take care of the
signing part
```sh
$ sudo bash -c 'cat << EOF > /etc/kernel/postinst.d/zz-sign-kernel
#!/bin/bash
set -e

# Path to your signing key and certificate
SIGN_KEY="/var/lib/dkms/mok.key"
SIGN_CERT="/var/lib/dkms/mok.crt"

# The kernel image to sign
KERNEL_IMAGE="\$2"

if [ -f "\$KERNEL_IMAGE" ] && [ -f "\$SIGN_KEY" ] && [ -f "\$SIGN_CERT" ]; then
    echo "Signing kernel image: \$KERNEL_IMAGE"
    sbsign --key "\$SIGN_KEY" --cert "\$SIGN_CERT" --output "\$KERNEL_IMAGE.signed" "\$KERNEL_IMAGE"
    
    # Replace the unsigned kernel with the signed one
    mv "\$KERNEL_IMAGE.signed" "\$KERNEL_IMAGE"
    
    echo "Kernel signed successfully"
else
    echo "Warning: Could not sign kernel - missing kernel image or signing keys"
fi

exit 0
EOF'
$ sudo chmod +x /etc/kernel/postinst.d/zz-sign-kernel
```
13. Now you should be able to install your new kernel
```sh
$ sudo apt install ../linux-image-6.17.8-loop0-rt_6.17.8-13_amd64.deb
```
14. Reboot and check if your new kernel worked, if you can't boot just go to the advanced section on grub screen 
and you should be able to boot your old kernel

Enjoy! If you find any issue in the instructions, please let me know so I can fix it, thanks!

## References
* [Debian Kernel Handbook](https://kernel-team.pages.debian.net/kernel-handbook/index.html)
* [Debian Secure Boot](https://wiki.debian.org/SecureBoot#MOK_-_Machine_Owner_Key)