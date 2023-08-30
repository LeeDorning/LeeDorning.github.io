---
layout: post
title: Hack The Box - Fawn
date: 2022-07-14
---

Write up on the box Fawn, from the Hack The Boc Starting Point - Tier 0.

&nbsp;

---

&nbsp;

# Fawn

On to box 2 of the Hack The Box Starting Point we go! This box is based around FTP (File Transfer Protocol) so kicks off with the same basic style of questions we had in Meow, such as what FTP stands for, the port it uses etc. We also get asked to provide information on the version number of FTP in use and an estimation of the Operating System of our target, which requires a new type of Nmap scan to be used.

&nbsp;
&nbsp;

## Enumeration

To find the version of FTP and the Operating System which are in use on the target machine we’re going to want to run an Nmap Service Scan. We can use our scan that we ran against the Meow box earlier and simply add the -sV flag to change this scan into a version scan:

~~~
nmap -sV -Pn -T4 -p- -v 10.129.64.9 -oN NmapTcpVersionScan.txt
~~~

![Nmap Results](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Hack%20The%20Box%20-%20Fawn/FawnNmapResults.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="400"}

&nbsp;

As you can see from the scan results, the device is running FTP version 3.0.3, with the base OS being UNIX. 

&nbsp;
&nbsp;

## Exploitation

Since FTP is the only service we’ve found on this device, clearly that is where we need to focus our attention to find the flag file. The easiest thing to test when faced with an FTP service is whether anonymous authentication is allowed by the service. Anonymous authentication allows you to connect and login to the service with the username: Anonymous and password: Anonymous OR blank. This is a running theme of these Tier 0 boxes… basic configuration errors which should always be checked. To connect to the FTP service running on the target and to find the flag file we’re going to go through a similar process to that which we followed for Meow:

~~~
ftp 10.129.64.9

username: anonymous

password: anonymous

ls

get flag.txt
~~~

![Nmap Results](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Hack%20The%20Box%20-%20Fawn/FawnFlag.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="400"}

&nbsp;

You can clearly see when I ran LS, the service listed all of the (1) files on the system, and running GET flag.txt transfers this file on to the testing device, on which we can run CAT flag.txt to display the flag string, and complete this box.

Nice!
