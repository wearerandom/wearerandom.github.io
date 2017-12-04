---
layout: post
title: "Raspberry Pi-neapple - WiFi Pineapple from RPi"
author: random
categories: [posts]
tags: [wifi, pineapple, raspberry, rpi, hacking, tools, mitm]
---
<p>The pals from <a target="_blank" href="https://www.hak5.org/">Hak5</a> built time ago the <a target="_blank" href="https://www.wifipineapple.com/">WiFi Pineapple</a>, a WiFi hacking tool with a ready web GUI, and the project <a target="_blank" href="http://www.fruitywifi.com/index_eng.html">FruityWifi</a> tried to make a <a target="_blank" href="https://www.raspberrypi.org/">Raspberry Pi version</a>.</p>

<a target="_blank" href="http://www.fruitywifi.com/index_eng.html">![Banner](/images/FruityWifi.jpg){: .center }</a>

<p>FruityWifi can be installed in any <a target="_blank" href="https://www.debian.org/">Debian</a> based system and there's an ARM version for <a target="_blank" href="https://raspbian.org/">Raspbian</a>, <a target="_blank" href="http://pwnpi.sourceforge.net/">Pwnpi</a> or <a target="_blank" href="https://www.offensive-security.com/kali-linux-arm-images/">Kali ARM</a>.</p>

<p>To install, we just need to download <a target="_blank" href="https://github.com/xtr4nge/FruityWifi/archive/master.zip">the zip</a> from <a target="_blank" href="https://github.com/xtr4nge/FruityWifi/">the repo</a>, unzip and run install-FruityWifi.sh. The installation script will install all the dependencies and setups. When done, we can launch a navigator pointing to <a target="_blank" href="https://localhost:8443">https://localhost:8443</a> and login as <b>admin:admin</b>. There's an HTTP version at port 8000 but we prefer to discard this option due security reasons.</p>

<p>Once logged in we can install modules from its tab. Modules list and their installation were scripted to be as transparent as possible, checking for updated versions on their repositories.</p>

<p>A RPi3 has built-in WiFi capabilities, so it's enough to lift up an open AP routing it through <a target="_blank" href="https://www.bettercap.org/">Bettercap</a>. Bettercap will intercept the POST queries and will perform the <a target="_blank" href="https://github.com/moxie0/sslstrip">sslstrip</a> downgrade when avaiable.</p>


