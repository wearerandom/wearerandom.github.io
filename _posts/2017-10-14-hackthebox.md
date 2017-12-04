---
layout: post
title: "HackTheBox.eu - A great free playground"
author: random
categories: [posts]
tags: [HackTheBox, training, challenges, virtual machines, hacking, playground, pen-testing labs]
---
<a target="_blank" href="https://www.hackthebox.eu/">HackTheBox.eu</a> is a VPN running vulnerable Virtual Machines for playing with your hacking skills.

<a target="_blank" href="https://www.hackthebox.eu/">![Banner](/images/hackthebox_panoramiczny.png){: .center }</a> 

The way to register in is <b>hacking the <a target="_blank" href="https://www.hackthebox.eu/invite">invite service</a> to get your own code</b>. There's no other way to get in.

<b>WE'RE NOT HELPING ANYONE TO GET AN INVITE CODE.</b> 
{:style="text-align: center;"}

Once you're registered, you can download the <i>.ovpn</i> file to connect via <a target="_blank" href="https://openvpn.net/">OpenVPN</a>. When connected you'll be able to connect to subnet <i>10.10.10.0</i> where the vulnerable machines are running.

There are always 2 targets in every machine: The <b>user</b> <i>(A file called user.txt at the user's desktop)</i> and the <b>system</b> <i>(File root.txt at /root/root.txt or at Windows Admin's desktop)</i>. When you get any of that files you can submit them in the <i>Machines</i> page to verify the flag and score your <i>Pwns</i>.

They also offer a <b>VIP service</b> for monthly £10.00 GBP or £100.00 GBP for a year subscription. The VIP service offers wider bandwith, machines hosted in more powerful hardware and access to Retired Machines.

After a time Active the machine becomes Retired, bringing space to a new Active machine to be released. The <b>20 Active Machines</b> are the machines that count the scoreboard, and the <b>Retired ones (15 right now)</b> are for fun and learning purposes.

Spoilers about Active Machines aren't allowed, but you can submit your own writeup of Retired Machines, and you can upload it to <a target="_blank" href="https://www.vulnhub.com/">VulnHub</a> if you're the machine's creator (Or you have permission from creator).

The machines come from the users. You can build and Submit your own vulnerable virtualized machine and it can become an Active Machine after being revised.

The scoreboard depends on the number of Users hacked, the Systems, the First Bloods <i>(Be the first submitting the flag after the release)</i> and <b>Challenges</b>.

The Challenges aren't virtual machines. They're on the next Categories: <b>Crypto, Stego, Pwn, Web, Misc and Forensics</b>. From cracking a zipfile to reversing a binary they offer many hours of fun and a lot of new things to learn.

Reaching higher scores brings your account status to a new level, depending on the solving progress of the Active Machines and Challenges. Starting from a <i>Noob</i> with the 0% you don't reach the <i>Script Kiddie</i> until you solve the 5%. Reaching the 20% you get the <i>Hacker</i> status, and <i>Pro Hacker</i> at 45%. Becoming <i>Elite Hacker</i> at 70% is a good target, because after this you can apply your CV to some <b>Job Offerings</b>. The higher your level the more job offerings you can apply. Over the 90% you become a <i>Guru</i> of HackTheBox, that's a pretty cool title.

All the Active Machines are avaiable to all kind of subscriber (Free or VIP) or level (Noob, Script Kiddie, Hacker, Pro Hacker, Elite Hacker or Guru).

One bad point is that you're hacking with other hackers and their job can meddle in your job. You can find the <i>/root/root.txt</i> file deleted, but you have a button to restart the machines on the web and it's easy to rerun the same way to the file. The hard job is to find that way.

Another frustrating point can be lose connectivity because someone issued a reset on the web. <b>There are an alert and a delay when you can cancel the reset</b>, but you can get violently stopped if machine gets a reset and you didn't pay attention. As before, <b>it's easy to reach the same point and continue</b>. Maybe even you can benefit from that reset because you didn't spot a dead service or someone has made a mess on the database.

To meet new friends and get some help when you're stuck, there is a <b>Shoutbox</b> that also works in the <i>HTB-CLI terminal</i> and they made a <a target="_blank" href="https://slack.com/">Slack</a> community with hundreds of hackers on it. The invitation to the Slack group is in the left navbar under the Forum link.

After solving some machines and challenges you unlock the option to create or manage your team, earning points in group and challenging other teams.

It's highly recommended for everyone: Newbies and hackers. Try the <a target="_blank" href="https://www.hackthebox.eu/invite">invite service</a> challenge to get in. It's funny!

Be polite, bring them a hello if you reach the crew and happy hacking!
