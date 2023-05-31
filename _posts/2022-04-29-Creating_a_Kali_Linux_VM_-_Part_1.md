---
layout: post
title: Creating a Kali Linux VM - Part 1 
date: 2022-04-29
---

Getting started with creating a Kali Linux VM for use while learning penetration testing.

---

&nbsp;

Here we go then, blog post number 1! I can’t say I ever thought I’d be much of a blogger, but inspiration has hit so let’s see how this goes. The first thing we’re going to cover is a bit of preparation for working through Hack the Box and Try Hack Me boxes; creating a Kali Linux VM and setting it up for use as a penetration testing box.

Both Try Hack Me and Hack The Box use OpenVPN packages to allow connections to their boxes, and you’ll need a penetration testing box to use to connect to, and test, the boxes on these platforms, hence why in this post I’m going to be going through the motions of creating a Kali Linux Virtual Machine to use with HTB and THM.

&nbsp;
&nbsp;

## Setting Up VMware

So, first thing’s first, we need to actually get set up with VMware so we have a program which we can use to build and run our VM… kind of an important step! There are a fair few different virtualisation options out there, think VirtualBox and Hyper-V, but in my personal experience VMware provides the best performance for your VM while the other options can provide some niggling issues that are difficult to overcome – I’ve had issues in the past with VirtualBox maxing out CPU on a VM as soon as Burp Suite is opened up, regardless of the specs of the host machine, which is obviously less than ideal.

So… setting up VMware. First off, you’re going to want to get yourself over to the VMware website and install the VMware workstation player 16 package that’s fit for your host OS. In my case this system is going to be on a Surface Book 3 running Windows 11, so I’m going to be downloading the windows version:

![Workstation Player Download](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_1/WorkstationPlayerDownload.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="500"}

&nbsp;

Since we’re working in cyber security, we have to follow our own best practices, which means checking that these downloads are legitimate and haven’t been tampered with (the CIA triad has entered the chat). There isn’t a specific checksum for the installer provided on VMware’s website, but for reference I ran the following command to find the SHA-256 hash of my installer:

*Get-FileHash ‘C:\Users\leedo\Downloads\VMware-player-full-16.2.3-19376536.exe*

*C05B61E8C70FD75A4E22984C3432C1F63480239D219937D13CAD73CC41DA1E09*

![Get-FileHash Screenshot](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_1/FileHash.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="500"}

&nbsp;

The ideal situation is that your checksum will match mine, but it’s possible that if by the time you’re doing this yourself, this blog is getting on a bit, then the file will have been updated and the checksum will be different. Once you’ve confirmed that the installer is legitimate, execute the installer to begin the installation of VMware workstation player 16 on your system. Click through the installer accepting the usual conditions of use, and ensure that when you reach the following page, you tick ‘Enhanced Keyboard Driver’:

![Vmware Enable Enhanced Keyboard](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_1/EnhancedKeyboard.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="300"}

&nbsp;

While the enhanced keyboard driver isn’t required for all virtual machines, I have found that in some cases it is required to allow you to actually type on the kali log in page, and obviously without that ability your VM is going to be a bit useless isn’t it! Keep clicking through the installer, taking note of what you’re agreeing to of course, and this will complete the installation of VMware workstation player 16. At this point it’s best to restart your computer to allow for the installation of the enhanced keyboard drivers.

When you first boot up VMware after restarting your machine you should be prompted to choose to either use the software for free for non-commercial use, or to enter a license key. I’ll only be using this software for personal use, but if you have a license key then feel free to enter it here, and hit continue to open up VMware:

![Vmware License Options](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_1/VmwareLicense.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="200"}

&nbsp;
&nbsp;

## Creating Your Kali Linux VM

Step 1 complete! We have VMware installed on our machine and ready to host a virtual machine, so now let’s get that machine created and ready to use. Thankfully, Kali provides their own VM images which can be downloaded direct from their website that are almost plug and play, with just a few simple steps to get them up and running. All of the different options for Kali Linux can be found at *https://www.kali.org*, with the VMware image files we are after downloaded using the VMware option down below:

![Kali VM Download](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_1/KaliVmDownload.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="300"}

&nbsp;

As with the installation of VMware earlier on, we need to ensure that the downloaded files have not been tampered with, and this time the Kali website does provide a checksum to compare yours to. Simply select ‘sum’ next to the VMware download button to display the current checksum, which in my case is:

*424E1011DB35E20F7670B310485AB3A37556FB7A2336D261F30A358F1A4E19BE*

![Kali VM SHA256](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_1/KaliVmSha256.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="300"}

&nbsp;

Running the same command as earlier on the downloaded file will output the checksum for the specific file downloaded to your machine, which for me outputs the following:

*Get-FileHash ‘C:\Users\leedo\Downloads\kali-linux-2022.1-vmware-amd64.7z’*

*424E1011DB35E20F7670B310485AB3A37556FB7A2336D261F30A358F1A4E19BE*

![Kali VM FileHash](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_1/KaliVmFileHash.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="500"}

&nbsp;

You can see that the expected checksum matches the actual checksum of my downloaded file, so we know that this download hasn’t been tampered with and should be safe to use. Since the downloaded file isn’t yet in a usable format we need to extract the actual files from this .7z file, for which I will be using 7-Zip (*https://www.7-zip.org*), but you can use whichever is your preferred tool for this. 

If, like me, you’re using 7-Zip, open up the software and navigate to the location of the downloaded Kali .7z file, before selecting extract. Complete the extract form including where to extract these files to and whether to add a password to the extracted files. For this example I’ve extracted the files into my downloads folder, but this clearly isn’t the best place if you’re going to be using this VM long term, so make sure to choose somewhere sensible:

![Kali VM 7-zip Extract](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_1/KaliVmExtract.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="400"}

![Kali VM 7-zip Options](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_1/KaliVmExtractOptions.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="350"}

&nbsp;

Success! We should now have a successfully extracted folder, within which there is a ready-to-go VMware virtual machine file. Now that’s complete, it’s time to jump back into VMware and get this VM up and running!

&nbsp;
&nbsp;

## Opening Your Kali VM

Once you’re back in VMware, select ‘Open Virtual Machine’ and navigate through the extracted folder to find your newly created VMware virtual machine file and open that VM:

![Kali VM Open VM](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_1/OpenVm.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="500"}

&nbsp;

This will create a VM in VMware which for all intents and purpose is ready to go, but there are still a couple of tweaks you can make to get the most out of your VM. Select the VM in VMware, and open ‘Edit Virtual Machine Settings’. Once the settings window pops up, verify the following is configured:

- *Memory: 4GB*
- *Processors: 4*
- *Hard Disk (SCSI): 80GB*
- *Network Adaptor: NAT*

![Kali VM Hardware Settings](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_1/VmHardware.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="300"}

&nbsp;

- *Name: Anything You Want!*
- *Version: Debian 10.x 64-bit*
- *Enhanced Keyboard: Use if Available*
- *Shared Folders: Disabled*

![Kali VM General Settings](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_1/VmSettings.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="400"}

&nbsp;

These are the recommended settings for running a Kali VM within VMware, although the Network Adaptor can be changed to suit your use case. If you are only using this VM within a virtual home lab with no internet connection required, it is best to select ‘Internal Only’, as this will prevent any risk of your VM being seen on the internet and used by others. Since we are using this VM for HTB and THM, we will need it to have internet connection, hence the use of NAT. This does present some risk however, as these Kali VM’s are not the most secure out of the box. That isn’t an issue for this post though, I’ll be going over how best to harden this VM in my next post. 

Now we’re ready to fire up our VM and log in for the first time! When you first boot up the VM you may be presented with the below pop-up, simply select ‘I copied it’ to launch the machine.

![Kali VM General Settings](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_1/VmCopied.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="300"}

&nbsp;

You should be passed through to the default Kali login screen, where you can login with the credentials Kali / Kali. As I mentioned earlier, enhanced keyboard allows me to type in this login screen, so if you are having issues with not being able to type in this login screen try closing the VM and altering the enhanced keyboard settings - Sometimes it needs to be required ,sometimes it needs to be turned off, and other times it needs to only be on use if required…it can be a bit of hassle but normally you can get it working.

And there we have it, a shiny new Kali Linux VM ready for use! It can technically be used as is, but its best to harden your machine before using it while connected to the internet, which I’ll be covering the next post. Fingers crossed this has been useful, and I’ll see you in the next one!
