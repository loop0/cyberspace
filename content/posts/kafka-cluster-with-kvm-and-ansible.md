---
title: "Kafka Cluster With Kvm and Ansible"
date: 2022-12-06T17:36:27-05:00
draft: true
---

* Ansible
* KVM
    * serial console to fix cloud-initramfs-growroot and cloud-init issue
    * qemu-img resize
* cloudinit
    * generate iso using cloud-utils cloud-localds
    * attach as as sata device
    * use debian generic image (not the generic cloud one, because it does not contain the hardware drivers to support cloud-init cdrom)
