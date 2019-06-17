---
layout: post
title: "FourAndSix:1 walkthrough"
author: random
categories: [posts]
tags: [CTF, walkthrough, FourAndSix, VulnHub, beginner]
---

<b>FourAndSix: 1</b> is a vulnerable machine available in <a href="https://www.vulnhub.com/entry/fourandsix-1,236/" target="_blank">VulnHub</a>. At this box, <b>Fred Wemeijer</b> brings us a <i>boot2root</i> CTF challenge. We must break into the system, get root and read the flag.

At first it's a pretty simple CTF and can be solved quickly, but we can dig further for educational purposes.

<h2>All is full of spoilers under here. If you want to face the challenge by yourself first you must stop reading here!</h2>
-----

<h3>Enumeration</h3>

We've targeted the VM and we need to find open services on the machine.

Scanning with nmap we find some interesting services running on.

```
# Nmap 7.70 scan initiated Tue Dec 25 21:54:54 2018 as: nmap -sT -p- -oN tcports.nmap 10.0.2.32
Nmap scan report for 10.0.2.32
Host is up (0.00087s latency).
Not shown: 65531 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
111/tcp  open  rpcbind
671/tcp  open  vacdsm-app
2049/tcp open  nfs

# Nmap done at Tue Dec 25 22:06:12 2018 -- 1 IP address (1 host up) scanned in 678.45 seconds
```

<h3>Recon</h3>

An <b>NFS</b> service is found, so let's take a look if we can access the served files.

```
$ showmount -e 10.0.2.32
Export list for 10.0.2.32:
/shared (everyone)
```

We mount the served folder and watch what's inside.

```
$ sudo mount -t nfs 10.0.2.32:shared mnt/
$ cd mnt/
$ ls -la
total 1046
drwxrwxrwx 2 root   root       512 Apr 29  2018 .
drwxr-xr-x 4 rand0m rand0m    4096 Jan  2 23:43 ..
-rw-r--r-- 1 root   root   1048576 Apr 29  2018 USB-stick.img
```

The file <i>USB-stick.img</i> seems juicy, so let's copy it to our local folder and dig a bit looking for something useful.

```
$ mkdir /tmp/4n6
$ cp USB-stick.img /tmp/4n6/.                                                
$ ls -lash1
total 1.1M
2.0K drwxrwxrwx 2 root   root    512 Apr 29  2018 .
4.0K drwxr-xr-x 4 rand0m rand0m 4.0K Jan  2 23:43 ..
1.1M -rw-r--r-- 1 root   root   1.0M Apr 29  2018 USB-stick.img

$ binwalk USB-stick.img                                                      

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------                                   
23040         0x5A00          PEM RSA private key
25088         0x6200          OpenSSH RSA public key
33280         0x8200          JPEG image data, JFIF standard 1.01                                                  
43520         0xAA00          PNG image, 257 x 196, 8-bit colormap, non-interlaced                                 
43783         0xAB07          Zlib compressed data, default compression                                            
49664         0xC200          JPEG image data, JFIF standard 1.01                                                  
59904         0xEA00          PNG image, 206 x 244, 8-bit colormap, non-interlaced                                 
60335         0xEBAF          Zlib compressed data, default compression                                            
70144         0x11200         JPEG image data, JFIF standard 1.01                                                  
80384         0x13A00         PNG image, 177 x 232, 8-bit colormap, non-interlaced                                 
80680         0x13B28         Zlib compressed data, default compression                                            
86528         0x15200         JPEG image data, JFIF standard 1.01                                                  
94720         0x17200         JPEG image data, JFIF standard 1.01                                                  
```

It looks like there is a RSA private key. This can be the key to login via SSH, so let's give it a watch.

```
$ dd if=USB-stick.img of=key bs=1 skip=23040 count=$((25088-23040))
2048+0 records in
2048+0 records out
2048 bytes (2.0 kB, 2.0 KiB) copied, 0.018798 s, 109 kB/s
$ dd if=USB-stick.img of=key.pub bs=1 skip=25088 count=$((33280-25088))
8192+0 records in
8192+0 records out
8192 bytes (8.2 kB, 8.0 KiB) copied, 0.0441432 s, 186 kB/s
$ cat key.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD6gjSEWyoPnQl3CpifosMo1Y+5exJ2uN1SQ2+JiJQhkVkGbbypxQ1ajJQUEdA0fDG5GVqTSi6HajTGkpXKEHz+9/+WZRtRap0K6t3UzkfT3Nf59TfN4OCKJot+LQihp5OZl5akzfj7bZGwOko6LJbOyia534uN2pEDIoQrRp0lVU89WhjKU6elqFvWTUp9QHtRyX8anQTn0xRf3lWExUOsgXUYZX7IcXVyOP49tEnSVEODjMjrReX6e8arqwP8qktEQMyU6+S4KXc2thmswcfaWSVWu0ELcRq4WEDxBGN/KYMVg/LsZ+kAlFaUs14xmTROJiovOwFOuHlLnVTXWanB user@fourandsix

$ cat key
-----BEGIN RSA PRIVATE KEY-----
HAHAHAHA THIS ONE IS ALSO DAMAGED6LDKNWPuXsSdrjdUkNviYiUIZFZBm28
qcUNWoyUFBHQNHwxuRlak0ouh2o0xpKVyhB8/vf/lmUbUWqdCurd1M5H09zX+fU3
zeDgiiaLfi0IoaeTmZeWpM34+22RsDpKOiyWzsomud+LjdqRAyKEK0adJVVPPVoY
ylOnpahb1k1KfUB7Ucl/Gp0E59MUX95VhMVDrIF1GGV+yHF1cjj+PbRJ0lRDg4zI
60Xl+nvGq6sD/KpLREDMlOvkuCl3NrYZrMHH2lklVrtBC3EauFhA8QRjfymDFYPy
7GfpAJRWlLNeMZk0TiYqLzsBTrh5S51U11mpwQIDAQABAoIBADsNYIXm26ZslWOb
etj+zFSe60+FBJg6Aeo3fV6FdK3pDnxmd/fpPLmgs/N7M4J72FjS8jgQX6GKVsCM
o4TLmDueiICSevsZT8XYEczth58Yy0zgEnSU0zmd1no68XLyBuhJBLj62PukG5jY
VNEb270JiFF+se4RnOeJRnDRJ5A58ZvJDkO9GdB1xdugkddVdrha8B1dECO1qn9M
enpbz3wc/N6SWeFxQ7gAKXubw80KYSx3n1W3frWS5EzdmHFZBvYvpBLLh417VNoE
F6m3cy+CcVwsVjgdQ7AkVF8Pn4WOKN9Epv3YFwE4Y8FtR/xHi4aZc+e2luRoeX65
ezd6LeUCgYEA/oRmiG1RswByZHYjdI1PzVv3P57+6Dtw+K9thG/ZkrRb+PcDcwup
```

Agh. It's <i>'damaged'</i>. At least now we know the username <i>user</i> from the public key. There are some tricky ways to reconstruct the corrupted key, so let's try!

<h3>Recovering damaged key</h3>

WORK IN PROGRESS

<h3>Exploiting bugs and misconfigurations</h3>


