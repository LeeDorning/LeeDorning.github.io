---
layout: post
title: Creating a Kali Linux VM - Part 2
date: 2022-05-13
---

Hardening your Kali Linux VM prior to using it for penetration testing.

&nbsp;

---

&nbsp;

Welcome back! In my previous post we covered how to get a new Kali Linux VM set up in VMware, ready to start hitting boxes on Try Hack Me and Hack The Box. However, this box is completely default at the moment, which means it is inherently insecure, so in this post we’re going to be fixing that and hardening our new box, as any good penetration tester would. 

For our use case of this VM, connecting to Hack The Box and Try Hack Me over the internet, there are a number of issues presented by using Kali out of the box in its default state, including:

- Running the system out of date
- Using the default username and password built into the VM image
- Using a permanently administrative user account
- Using the default SSH keys provided with the VM image

Each of these presents an avenue into your box that could be exploited by an attacker that discovers your VM on the internet, if that should happen. In this post we’re going to be walking through the steps you need to take to resolve these issues and harden your Kali VM to a sensible state. These aren’t all the issues that Kali has, not even close, but they are the most important to fix, since they are the most commonly seen. It’s important as a security professional that we don’t commit the cardinal sin of opening ourselves up to attack, especially when we’re using tools such as Kali for penetration tests, but following the steps in this post should help to keep you secure, and your reputation intact.

![Kali Hardening Meme](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_2/KaliHardeningMeme.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="500"}

&nbsp;
&nbsp;

## Updating Your Kali VM

Right then, let’s get into it. Keeping systems up to date is one of the foundations of security, and that’s no different for this VM, so updating and upgrading the system is going to be our first port of call. To do so, you’re going to want to run:

~~~
sudo apt update && sudo apt upgrade
~~~

![Updating and Upgrading Kali](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_2/UpdateAndUpgrade.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="500"}

&nbsp;

This is in essence two separate commands combined into one using the && to connect the two and run the second command once the first has completed. So let’s dig into what these commands are actually doing.

~~~
sudo apt update
~~~

This command is used to collect all of the current package information from all of the sources configured in /etc/apt/sources. This sources list does come pre-configured with Kali, but extra sources can be manually configured should they be necessary for specific tools you’re running on your system. This command is run first simply because the updated package information is necessary for Kali to know what programs or systems actually require an upgrade, and which do not. Without running an update of the packages first, your system will not be aware of any upgrades that are necessary and so won’t make any, and this is also how Kali is made aware of any new packages that are now built in, but previously were not. 

~~~
sudo apt upgrade
~~~

This is where the actual upgrading happens. Running this command leverages the updated packages list that the update command collected, and tells your system to upgrade any current or new packages to their latest software version. Running this command without any arguments, as we are in this example, will tell the system upgrade all packages on the system to their latest versions regardless of their current state, and to install all dependencies needed by these updated versions of Kali’s software.

And there you go, your Kali VM should be successfully updated to use the latest version of all software on the system. It is best practice to continue running these commands regularly to keep your system updated and free of the inevitable ‘out-dated software’ vulnerabilities that always pop-up. If you’re going to be updating and upgrading your VM after it has been in use for a while it is best practice to take a snapshot of the VM before the update, just in case any packages corrupt and cause issues with the VM itself.

&nbsp;
&nbsp;

## Creating a New User

Okay, we’re now up to date and in a better position than where we started, but we’re still using the built in account with default credentials and administrative access, which is just horrible security practice. If we left the box in this state, anybody would be able to find these default credentials on the kali website, log in to our VM, and have admin access… not a position we ever want to be in. So what we do about that? You guessed it, time to create a new user account and secure the default account. First off, let’s create a new user with non-root access:

~~~
sudo useradd -m Lee
~~~

![Add New Kali User](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_2/UserAdd.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="500"}

&nbsp;

This is going to create a new user with non-admin rights, while the -m flag is going to create a home directory for this user which we can work out of in the future. While this is a good start there is currently no password on the account, not exactly an ideal place to leave the account, so let’s run this next command to add a password to the account:

~~~
sudo passwd Lee
~~~

![Change New User Password](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_2/Passwd.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="500"}

&nbsp;

There aren’t any specific password requirements built into Kali, but it is important that the password you choose for this account is secure. The best option is to use a randomly generated password, and store this password in a secure place like a password manager. However, if you can’t do so it is best to create a password of a minimum of 12 characters, including a random mix of uppercase, lowercase, numbers and symbols. 

Now we’re getting somewhere, our new account is set up as a non-admin, has a secure password set for login, and is ready to use. Still, it is likely that for regular penetration testing activity this account is going to need some kind of admin access, so we’re going to want to grant this account a certain amount of admin access. To achieve this, we can run the following commands to add the user to the ‘sudo’ group, which will allow for elevation of privilege by running the sudo command and re-entering the user account password:

~~~
sudo usermod -a -G sudo Lee
~~~

![Add New User Sudo Rights](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_2/Usermod.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="500"}

&nbsp;

That’s our user account now in the sudo group, and given access to certain admin rights. Now there is just one final thing to set up; The user account needs to be specified a particular terminal shell. For my account I prefer to use the bash shell, located at /bin/bash, so the command needed is:

~~~
sudo chsh /bin/bash Lee
~~~

![Set New User Terminal Shell](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_2/SetBashShell.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="500"}

&nbsp;

That’s our new user account all ready to go. We’ve successfully created the account, added a password, added it to the sudo group to allow admin access, and finally specific the shell the new account will be using. 

Now we need to move on to securing the built in kali account, and the first step is to log out of the built in account and log back in with your newly created user account. Once you’re logged back in, open up your terminal and we’re going to be changing the password of the built in account to something heavily secure:

~~~
sudo passwd kali
~~~

![Change Default Kali Account Password](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_2/PasswdKali.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="500"}

&nbsp;

As far as securing the built in account, that is about as far as you can go, so make sure that whatever password you set is complex, and heavily secure. That’s step 2 complete now then, we’ve secured the built in user account and created a new user account of our which runs as a standard user, but has access to the sudo group to run with admin privileges if needs be.

&nbsp;
&nbsp;

## Generating New SSH Keys

And now we’re onto our final steps of securing our Kali VM, generating new SSH keys to replace the default keys provided in the Kali image. Every Kali box from the same build will have the same default keys, which obviously poses security risks if it is left as is, so we need to remove these default SSH keys and generate new, unique keys for our machine. To do so, we’re gong to want to first navigate into the /etc/ssh/ directory, create a new folder within this directory for us to store the default keys within, and finally move the default keys into this folder:

~~~
cd /etc/ssh/
sudo mkdir default_kali_keys
sudo mv ssh_host_* default_kali_keys
~~~

![Removing Old SSH Keys](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_2/OldSshKeys.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="500"}

&nbsp;

Once that’s done we want to reconfigure the built in openssh-server, to generate new SSH keys in the /etc/ssh/ directory. Once they are generated, we will want to run md5sum on these keys, to view their checksums. We then want to compare these checksums against those of the default kali keys, to ensure we do indeed have new, unique keys in our /etc/ssh/ directory:

~~~
sudo dpkg-reconfigure openssh-server
md5sum ssh_host_*
~~~

![Generate New SSH Keys](https://github.com/LeeDorning/LeeDorning.github.io/blob/main/images/Creating_a_Kali_Linux_VM_-_Part_2/NewSshKeys.png?raw=true){:style="display:block; margin-left:auto; margin-right:auto"}{: width="500"}

&nbsp;

As you can see, I now have new SSH keys generated within my folder which returned the above checksums. You will want to run md5sum with your sudo privileges to return the checksum for all of the keys, rather than just the 3 checksums I have above. This was done purely to not advertise my SSH keys checksums, although I generated new keys again once this screenshot was taken… you can’t be too careful! Once you have run this command, navigate into the default_kali_keys folder and run the same command to gather the checksums of your default keys. Compare these two sets of keys to confirm they are different, and if so you’re done!

There we have it, you have successfully secured your Kali VM, and it’s now in a worthy state of being used for penetration testing and keeping your device secure. That concludes this mini-series of getting set up with a VMware VM and securing it for use with THM and HTB. I hope its been interesting, useful, entertaining, or even all of the above, and if so feel free to come back for future posts. See you around!
