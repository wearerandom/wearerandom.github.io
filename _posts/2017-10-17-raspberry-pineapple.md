---
layout: post
title: "Raspberry Pi-neapple - WiFi Pineapple from RPi"
author: random
categories: [posts]
tags: [wifi, pineapple, raspberry, rpi, hacking, tools, mitm]
---
<p>The pals from <a target="_blank" href="https://cnhv.co/693i">Hak5</a> built time ago the <a target="_blank" href="https://cnhv.co/693l">WiFi Pineapple</a>, a WiFi hacking tool with a ready web GUI, and the project <a target="_blank" href="https://cnhv.co/693r">FruityWifi</a> tried to make a <a target="_blank" href="https://cnhv.co/696t">Raspberry Pi version</a>.</p>

<a target="_blank" href="https://cnhv.co/693r">![Banner](/images/FruityWifi.jpg){: .center }</a>

<p>FruityWifi can be installed in any <a target="_blank" href="https://cnhv.co/694o">Debian</a> based system and there's an ARM version for <a target="_blank" href="https://cnhv.co/695r">Raspbian</a>, <a target="_blank" href="https://cnhv.co/695p">Pwnpi</a> or <a target="_blank" href="https://cnhv.co/695c">Kali ARM</a>.</p>

<p>To install, we just need to download <a target="_blank" href="https://cnhv.co/695x">the zip</a> from <a target="_blank" href="https://cnhv.co/6962">the repo</a>, unzip and run install-FruityWifi.sh. The installation script will install all the dependencies and setups. When done, we can launch a navigator pointing to <a target="_blank" href="https://localhost:8443">https://localhost:8443</a> and login as <b>admin:admin</b>. There's an HTTP version at port 8000 but we prefer to discard this option due security reasons.</p>

<p>Once logged in we can install modules from its tab. Modules list and their installation were scripted to be as transparent as possible, checking for updated versions on their repositories.</p>

<p>A RPi3 has built-in WiFi capabilities, so it's enough to lift up an open AP routing it through <a target="_blank" href="https://cnhv.co/696b">Bettercap</a>. Bettercap will intercept the POST queries and will perform the <a target="_blank" href="https://github.com/moxie0/sslstrip">sslstrip</a> downgrade when avaiable.</p>

