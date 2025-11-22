---
title: "How to check supported protocols and ciphers on HTTPS"
date: 2020-04-04
tags: ["security"]
---

In case you need to check which protocols and ciphers are supported on a HTTPS endpoint you can use nmap for that. Just use the following command, replacing `loop0.sh` for the desired domain.

```sh
nmap --script ssl-enum-ciphers -p 443 loop0.sh
```

The previous command should give you an output like this:
```sh
Nmap scan report for loop0.sh (104.24.102.73)
Host is up (0.022s latency).
Other addresses for loop0.sh (not scanned): 104.24.103.73 2606:4700:3037::6818:6749 2606:4700:3033::6818:6649

PORT    STATE SERVICE
443/tcp open  https
| ssl-enum-ciphers: 
|   TLSv1.0: 
|     ciphers: 
|       TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA (prime256v1) - A
|       TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA (prime256v1) - A
|     compressors: 
|       NULL
|     cipher preference: server
|   TLSv1.1: 
|     ciphers: 
|       TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA (prime256v1) - A
|       TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA (prime256v1) - A
|     compressors: 
|       NULL
|     cipher preference: server
|   TLSv1.2: 
|     ciphers: 
|       TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 (prime256v1) - A
|       TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256 (prime256v1) - A
|       TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256-draft (prime256v1) - A
|       TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA (prime256v1) - A
|       TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256 (prime256v1) - A
|       TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 (prime256v1) - A
|       TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA (prime256v1) - A
|       TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384 (prime256v1) - A
|     compressors: 
|       NULL
|     cipher preference: server
|_  least strength: A

Nmap done: 1 IP address (1 host up) scanned in 5.49 seconds
```