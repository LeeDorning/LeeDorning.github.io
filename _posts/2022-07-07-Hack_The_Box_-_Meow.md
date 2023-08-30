---
layout: post
title: Hack The Box - Meow
date: 2022-07-07
---

Write up on the box Meow, from the Hack The Box Starting Point - Tier 0.

&nbsp;

---

&nbsp;

# Meow

No, I don’t have a cat that just wandered across my keyboard (ahaha… ugh. that was a Dad joke and a half), Meow is our first box from Tier 0 of the starting point. This box is (as you’d expect) incredibly simple and the main idea is to get used to how you connect to Hack The Box and work through their boxes, so this isn't going to be a very laborious post at least!

&nbsp;
&nbsp;

## Connection

First up is getting connected to the system for the first time, which once you know how to do it is a really simple experience. Pretty much all you need to do is find the ‘connect’ button, choose the correct options and download your OpenVPN connection file:

![VPN Connection](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Hack%20The%20Box%20-%20Meow/MeowVpnConnectiong.png?raw=true)){:style="display:block; margin-left:auto; margin-right:auto"}{: width="600"}

&nbsp;

Once that file is downloaded, you just need to fire up your terminal and run the following command, swapping out my OpenVPN package name for yours (assuming you downloaded the OpenVPN file to your current directory):

~~~
sudo openvpn ./starting_point_USERNAME.ovpn
~~~

&nbsp;

This should initiate an OpenVPN connection from your box into the Hack The Box environment and will allow you to gain access to all of the boxes that are offered on the platform. You’re going to want to make sure you keep the terminal window which OpenVPN is running within open for as long as you want to use HTB, otherwise you’ll lose your connection.

&nbsp;
&nbsp;

## Enumeration

Okay, we’re connected to the system and ready to get into the actual meat and bones of this box. It starts out with some pretty basic questions all around testing connectivity of the system (PING is your friend!) until we start looking at some actual testing. This box is mainly about enumeration and testing of Telnet, which is a very old, insecure protocol, which should catch any half-decent tester’s eye as soon as it pops up. 

The first step of every test (well, most of the time) is to confirm you can contact the target machine and then run a port scan against the device to gather some further information:

~~~
ping 10.129.234.140
nmap -Pn -T4 -p- -v 10.129.234.140 -oN NmapTcpAllPorts.txt
~~~

&nbsp;

I’m running this scan with ping turned off (-Pn) since we’ve just ping tested the device, aggressive timing (-T4) to speed up the scan, all 65535 ports being scanned (-p-), some verbosity (-v) to show progress of the scan as it goes along, and finally outputting all of the results into the file NmapTcpAllPorts.txt. This is my preferred preliminary scan, but there are so many different options with Nmap that it’s mainly about finding what you’re comfortable with and what works for you. My scan spat out the following open ports:

~~~
telnet: 23 / TCP
~~~

&nbsp;
&nbsp;

## Exploitation

This should kick off some alarm bells, since Telnet is well-known as an outdated, insecure protocol. My first port of call when I find an open Telnet service is to simply connect and see what we can do:

~~~
telnet 10.129.234.140 23
~~~

&nbsp;

In typical, insecure, Telnet fashion there aren’t any credentials configured on the service, so we’re given root access without having to provide a username or password. Now that we’re into this Telnet service the final step is getting hold of the flag.txt file somewhere on the remote device. We’ll start off by trying to see where exactly we are in this device’s file system and what files are in this directory with us:

~~~
ls -la
~~~

&nbsp;

My results from this command are below and you can clearly see that the flag.txt is located in the same directory that I am currently logged into as root. 

![Ls -La Result](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Hack%20The%20Box%20-%20Meow/MeowLsLa.png?raw=true)){:style="display:block; margin-left:auto; margin-right:auto"}{: width="600"}

&nbsp;

Since we are in the same directory as this file, and from the file permissions (-rw-r—r—) we know we can read this file, it’s as simple as running cat against the file to output the flag.txt contents, which can then be submitted to the Hack The Box system to complete the box!

![Ls -La Result](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Hack%20The%20Box%20-%20Meow/MeowCatFlag.png?raw=true)){:style="display:block; margin-left:auto; margin-right:auto"}{: width="600"}

&nbsp;

Result!
