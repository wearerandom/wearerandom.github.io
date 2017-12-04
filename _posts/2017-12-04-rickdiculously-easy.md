---
layout: post
title: "RickdiculouslyEasy:1 walkthrough"
author: random
categories: [posts]
tags: [rickdiculouslyeasy, blackbox, walkthrough, writeup, guide, beginner, ctf]
---

<b>RickdiculouslyEasy: 1</b> is a box available at <a href="https://www.vulnhub.com/entry/rickdiculouslyeasy-1,207/">VulnHub</a>. At this box, Luke brings us 130 points hidden in different flags, as we can see in <a target="_blank" href="https://www.vulnhub.com/entry/rickdiculouslyeasy-1,207/#description">description</a>. Let's find them!

![Description](/images/rickdiculous1.png){: .center }

After downloading and importing, we boot the machine in our Lab network. When boot has completed we can see the IP it took from DHCP service.

![Bootscreen](/images/rickdiculous2.png){: .center }

<h2>All is full of spoilers under here. If you want to face the challenge by yourself first you must stop reading here!</h2>

Navigating to the URL shown in boot screen we find the first flag.

![Flag1](/images/rickdiculous3.png){: .center }

Ok! So the game has started! We've scored 10 of 130. Let's start by enumerating the services running on the server, so we run nmap against all TCP ports to check where we can connect to.

![Nmap](/images/rickdiculous4.png){: .center }

Nmap reveals 7 services running, some of them are marked as 'unknown'. A further enumeration step is to check which service and version is running in the host. We need more info.

![Versions](/images/rickdiculous5.png){: .center }

I marked in yellow some interesting data. At first, we found a service running at port 13337 that serves a flag (We're 20/130). The service running at port 60000 seems to be some kind of shell. We must try.

![Pickle!](/images/rickdiculous6.png){: .center }

So here we have another flag (30/130) and we've not permissions to change directory, but there are a lot of things we have to try. There are a lot of services running in the host, and the port 80 <i>(HTTP)</i> is always an interesting thing. 

The web server only shows the "MORTY'S COOL WEBSITE". It only displays the text "It's not finished yet ok. Stop judging me.", and checking the source code doesn't reveal anything. Let's scan the server with Nikto.

![Nikto](/images/rickdiculous7.png){: .center }

Nikto finds interesting the directory <i>/passwords/</i>. I'll agree with it. At this folder there are 2 files: <i>FLAG.txt</i> and <i>passwords.html</i>.

![Nikto](/images/rickdiculous8.png){: .center }

With this flag we're at 40/130. At <i>/passwords/passwords.html</i> we find a comment with a supposed password. We must keep it in mind.

![winter](/images/rickdiculous9.png){: .center }

Maybe it's a password for the FTP server. But at least we must try to login as <i>anonymous</i> as we didn't try before.

![Anonymous FTP](/images/rickdiculous10.png){: .center }

Ok! So the score is 50/130 at this moment! At port 80 we need to dig a bit further to find a way to interact with the server. Dirb is a very nice tool to take a look for directories and hidden panels.

![dirb](/images/rickdiculous11.png){: .center }

Using dirb we can check there is a <i>robots.txt</i> that Nikto didn't notify (A negative point to Nikto). It's time to check what's hidden for search engines spiders and robots.

![dirb](/images/rickdiculous12.png){: .center }

Thanks to <i>robots.txt</i> we can find the "MORTY'S MACHINE TRACER MACHINE". If we test the service we can try to guess how it works and try to exploit it to run commands on the server.

![traceroute](/images/rickdiculous13.png){: .center }

It looks like it takes the argument and runs the traceroute program against that argument. So when we put 8.8.8.8 as the argument the page runs the <i>traceroute 8.8.8.8</i> command in shell and then shows the result at the webpage.

Taking that in mind we can try to concatenate commands in the traceroute script. The '&' symbol can be problemthic as it passes in the URL as a GET request, but using ';' we can run commands in the server.

![traceroute](/images/rickdiculous14.png){: .center }

The <i>whoami</i> command tells we can run commands as the <i>apache</i> user. If we read the <i>/etc/passwd</i> file we can find the usernames that login in the system.

![cat](/images/rickdiculous15.png){: .center }

Ha-ha. Very funny. The command <i>cat</i> was replaced by a command that displays a cat. But no problem! We can use many other commands to display text as <i>head</i>, <i>tail</i>...

![passwd](/images/rickdiculous16.png){: .center }

So there are 3 users that we can try to use: <b>RickSanchez</b>, <b>Morty</b> and <b>Summer</b>. We must try the previous password we found (winter) to SSH that users.

When trying to connect to SSH the server always refuses the connection. If we connect via Telnet we get a Banner and the connection closes. That's not the way a SSH receives the connections (Telnet isn't encrypted), so let's try to connect to the port nmap previously marked as OpenSSH 7.5: The port 22222.

![SSH](/images/rickdiculous17.png){: .center }

Allright! So we're on a SSH connection with credentials Summer:winter (Quite bad password)! Now what? It's time to start searching for flags here. The cat joke remembers us that we're not focused.

![Flag](/images/rickdiculous18.png){: .center }

Owning this flag scores 60 of 130. We're near the 50%. If we try to run the binary <i>safe</i> we're asked to run it with some arguments. If we run it with arguments we get some "decrypted" content. Must keep this binary in mind.

![safe](/images/rickdiculous19.png){: .center }

At this point we should go to other user's home directory. At /home/Morty we find 2 other interesting files: A zip and an image. A good idea is to download the files to analyze them locally.

![sftp](/images/rickdiculous20.png){: .center }

A simple analysis using strings reveals a the password to decrypt the journal.txt.zip file.

![strings](/images/rickdiculous21.png){: .center }

This journal reveals a lot of good info. At first we find that in the file there's <i>a password to a 'safe' place, or something like that</i>. Thanks, Morty!

We also have a flag, so the score now is 80/130. Over 50% completed. On the right way.

![journal](/images/rickdiculous22.png){: .center }

That 'safe' place can be related to the 'safe' binary at /home/Summer, so we must try the flag as key.

![secureOK](/images/rickdiculous23.png){: .center }

Now we have another flag (Score 100/130) and a hint for the Rick's password. It's composed of 1 upper letter, 1 digit and a word of Rick's old band's name. It also talks about sudo, so maybe here's where the root access is (Summer isn't in the sudoers group).

A bit of research gives us the band's name: <i>The Flesh Curtains</i>, so we need to compose the password dictionary to bruteforce the <i>RickSanchez</i> SSH account. This can be done using <i>crunch</i>.

![bruteforce](/images/rickdiculous24.png){: .center }

After trying the combination of 1 uppercase, 1 digit and <i>"The"</i>, we need to build another dictionary with another word and try it again with hydra.

The clues for the password dictionaries makes us find it at the 3rd dictionary, and knowing it's the sudoer account we've gained root access, so we must now get all possible flags remaining.

![hydra](/images/rickdiculous25.png){: .center }

Once logged in and run sudo we're able to look into <i>/root</i>, where there's another flag. But to get the flag we can see another cat if we're distracted <i>(Ha ha)</i>.

![root](/images/rickdiculous26.png){: .center }

That flag brings us the last 30 points, the Score now is 130/130, so we've completed the box. I hope you enjoyed this writeup and maybe helped you in some way.

I'll try to upload more writeups soon.