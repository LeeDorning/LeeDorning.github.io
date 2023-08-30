---
layout: post
title: Hack The Box - Dancing
date: 2022-07-21
---

Write up on the box Dancing, from the Hack The Box Starting Point - Tier 0.

&nbsp;

---

&nbsp;

# Dancing

Off we go again then, this time hitting the box ‘Dancing’, which is targeted on basic enumeration and exploitation of SMB / Samba. There’s a couple of the same basic questions about the service in general, but this time there’s also a few specific syntax questions to guide you through rooting the box.

&nbsp;
&nbsp;

## Enumeration

Shock horror, we’re going to run an Nmap port scan first… who’d have guessed it

~~~
nmap -Pn -T4 -p- -v 10.129.137.226 -oN NmapTcpAllPorts.txt
~~~

&nbsp;

This time the target box gives back a fair few results, rather than just the single port that we saw in previous boxes:

![Nmap Results](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Hack%20The%20Box%20-%20Dancing/DancingOpenPorts.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="300"}

&nbsp;

These ports are all indicative of the target device most likely being a Windows host, and the port we are most interested in with this particular target is 445, which Nmap states is the Microsoft-ds service but is in fact SMB / Samba. Now that we know the open ports on this box we are first asked how to list the contents of the SMB share on this device, and this can be done using the -L flag:

~~~
/usr/bin/smbclient -L 10.129.137.226
~~~

![Smb Client Results](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Hack%20The%20Box%20-%20Dancing/DancingSmbClient.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="300"}

&nbsp;
&nbsp;

## Exploitation

We can see that this device is hosting 4 shares, with one in particular of interest: WorkShares. This share is of particular interest because it doesn’t end with a $, which indicates it has been manually created and could potentially be mis-configured and thus vulnerable. Now that we’ve found this share of interest let’s have a go at connecting to it. Since we don’t know any credentials for the share all we can try is to connect without credentials (-N specifies we don’t want to enter a password):

~~~
/usr/bin/smbclient -N \\\\10.129.137.226\\WorkShares
~~~

&nbsp;

Worryingly (if this was a live system anyway), we are actually allowed to login to this share without any credentials. If you have any issues with your login not working at this point, ensure you’re not running the connection command as the root account - this was causing me issues but everything was sorted by running the command as a standard user account. 

Now that we’re inside this share, we want to try and find where exactly the flag.txt file is located. First off, we want to view what files or directories are stored in our current directory, in the hope that flag.txt is nice and simple to get hold of:

~~~
Dir
~~~

![Dir Results](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Hack%20The%20Box%20-%20Dancing/DancingDir.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="300"}

&nbsp;

No such luck, flag.txt isn’t in our directory. However, we can see there are two sub-directories which we can move into and investigate to see if the flag file is in there instead. To move into each of these directories and view their contents we’re going to run:

~~~
cd Amy.J
ls
cd ../James.P
ls
~~~

![Dir Results](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Hack%20The%20Box%20-%20Dancing/DancingLs.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="500"}

&nbsp;

In the directory Amy.J there is only ‘worknotes.txt’, which we can GET to transfer it to the testing device just in case, but as soon as we move into the James.P directory the flag.txt file pops up. GET that flag.txt file, open it up, and submit the flag to HTB to complete this box!

And there we have it, that's box number 3 of the Starting Point ticked off. They've been a good way to get stuck into things without it getting too difficult, but now we've got a good idea of how the boxes work it's going to be fun to get into some harder challenges moving forward. See you in the next one!
