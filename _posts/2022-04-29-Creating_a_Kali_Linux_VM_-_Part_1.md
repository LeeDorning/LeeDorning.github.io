---
layout: page
title: Creating a Kali Linux VM - Part 1 
date: 2022-04-29
---

Getting started with creating a Kali Linux VM for use while learning penetration testing.

Here we go then, blog post number 1! I can’t say I ever thought I’d be much of a blogger, but inspiration has hit so let’s see how this goes. The first thing we’re going to cover is a bit of preparation for working through Hack the Box and Try Hack Me boxes; creating a Kali Linux VM and setting it up for use as a penetration testing box.

Both Try Hack Me and Hack The Box use OpenVPN packages to allow connections to their boxes, and you’ll need a penetration testing box to use to connect to, and test, the boxes on these platforms, hence why in this post I’m going to be going through the motions of creating a Kali Linux Virtual Machine to use with HTB and THM.

### Setting Up VMware

So, first thing’s first, we need to actually get set up with VMware so we have a program which we can use to build and run our VM… kind of an important step! There are a fair few different virtualisation options out there, think VirtualBox and Hyper-V, but in my personal experience VMware provides the best performance for your VM while the other options can provide some niggling issues that are difficult to overcome – I’ve had issues in the past with VirtualBox maxing out CPU on a VM as soon as Burp Suite is opened up, regardless of the specs of the host machine, which is obviously less than ideal.

So… setting up VMware. First off, you’re going to want to get yourself over to the VMware website and install the VMware workstation player 16 package that’s fit for your host OS. In my case this system is going to be on a Surface Book 3 running Windows 11, so I’m going to be downloading the windows version:

![Workstation Player Download](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_1/WorkstationPlayerDownload.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}


Since we’re working in cyber security, we have to follow our own best practices, which means checking that these downloads are legitimate and haven’t been tampered with (the CIA triad has entered the chat). There isn’t a specific checksum for the installer provided on VMware’s website, but for reference I ran the following command to find the SHA-256 hash of my installer:

***Get-FileHash ‘C:\Users\leedo\Downloads\VMware-player-full-16.2.3-19376536.exe*** {: style="text-align: center;"}

***C05B61E8C70FD75A4E22984C3432C1F63480239D219937D13CAD73CC41DA1E09*** {: style="text-align: center;"}

![Get-FileHash Screenshot](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_1/FileHash.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}


The ideal situation is that your checksum will match mine, but it’s possible that if by the time you’re doing this yourself, this blog is getting on a bit, then the file will have been updated and the checksum will be different. Once you’ve confirmed that the installer is legitimate, execute the installer to begin the installation of VMware workstation player 16 on your system. Click through the installer accepting the usual conditions of use, and ensure that when you reach the following page, you tick ‘Enhanced Keyboard Driver’:

![Vmware Enable Enhanced Keyboard](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_1/EnhancedKeyboard.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}


While the enhanced keyboard driver isn’t required for all virtual machines, I have found that in some cases it is required to allow you to actually type on the kali log in page, and obviously without that ability your VM is going to be a bit useless isn’t it! Keep clicking through the installer, taking note of what you’re agreeing to of course, and this will complete the installation of VMware workstation player 16. At this point it’s best to restart your computer to allow for the installation of the enhanced keyboard drivers.

When you first boot up VMware after restarting your machine you should be prompted to choose to either use the software for free for non-commercial use, or to enter a license key. I’ll only be using this software for personal use, but if you have a license key then feel free to enter it here, and hit continue to open up VMware:

![Vmware License Options](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_1/VmwareLicense.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}

