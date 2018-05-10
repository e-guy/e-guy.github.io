---
layout:     post
title:      test
date:       2018-5-2
author:     e-guy
summary:    Carte Noire is a dark blog theme for Jekyll focusing on a clear reading experience.
categories: test
thumbnail:  heart
tags:
 - test
 - pos
---


# Setup accounts

*Time to complete: About ?-? minutes*

## GitHub
 
**[Create a GitHub account](https://github.com/join)**

## Slack

Please choose the same handle you used for your GitHub account, or provide some other distinguishing feature like your name or photo so the team can know who you are. 

**Please ask in [#help](https://runswift.slack.com/messages/help) if you have any questions.**

## Virtual Machine Preparation

### Choose a VM provider

On Windows, good VM's are VMWare (which CSE students get for [free](http://taggi.cse.unsw.edu.au/FAQ/VMware_Academic_Program/)) and [VirtualBox](https://www.virtualbox.org/wiki/Downloads). On Mac OSX, VMWare Fusion is good (also [free](http://taggi.cse.unsw.edu.au/FAQ/VMware_Academic_Program/)). [VirtualBox](https://www.virtualbox.org/wiki/Downloads) is also required if you need to compile software in the Aldebaran-provided NaoVM.

To setup rUNSWift on a VM, you may use the provided VM image on the rUNSWift server (only available behind the CSE firewall). If you would like to setup your own VM, see the instructions under [Manual VM Setup](#manual-vm-setup). 

### Obtaining VM Software
Students may obtain a free version of VMWare Workstation 12 from [OnTheHub](https://e5.onthehub.com/d.ashx?s=oo30qimh1a). This is the recommended virtualisation software in which the image has been verified to work. [VirtualBox](https://www.virtualbox.org/wiki/Downloads) works very well too (but you will need to build your own VM image). The same goes for other VM software.

# Virtual Machine Setup

**NB: If you would like to be part of the travelling team in any given year, it is strongly recommended you skip down to the [Manual VM Setup](#manual-vm-setup) section and follow that.**

## Provided VM

*Time to complete: About ?-? minutes*

### Obtaining the VM Image
The VM image is located on the runswift server. To obtain it, you can use scp, as follows
```
scp runswift@runswift.cse.unsw.edu.au:~/vm_image.zip ~
```
The password for the server is found on the [[Passwords]] page

The above command places the zip file in your home directory. If you're using Windows, you can use a tool such as WinSCP to download the file, or a bash shell such as the Linux Subsystem for Windows 10.

Unzip the file to a location of your choice. You should see three files:
* runswift.ovf
* runswift.vmdk
* runswift.mf

### Importing the VM
The following steps are for VMware Workstation 12. Steps for other software should be similar.
1. Navigate to the home tab.
2. Press "Open a new virtual machine".
3. In the file browser, navigate to the folder where you unzipped ```vm_image.zip```, and select ```runswift.ovf```.

VMware should now begin importing the VM. You may see an error along the lines of,
```
runswift.ovf did not pass OVF specification conformance...Click Retry to relax OVF specification...
```
Clicking Retry should allow the import to proceed.

The VM has been setup with the following hardware:
* 30 GB Hard Disk
* 3 GB RAM
* 2 processors, with 2 cores each.

You may wish to change the hardware settings, especially if your device has additional processors. This may improve build times.

After you're done, power on the VM. You should arrive at the Ubuntu login screen. You can login using the password `runswift`. You are advised to change this password using the `passwd` command.

### Final Steps
In the VM, open the terminal using `Ctrl + Alt + t`. Change directory to `~/rUNSWift`. You should see the rUNSWift files here. Try opening offnao by navigating to `build-release` and typing `offnao`.

You will need to do some GitHub configuration, as detailed under [Setting up a GitHub account](#setting-up-a-github-account). This is essential in allowing you to pull the latest code for the repository.

After you're done, make sure you read the [Post Setup](#post-setup) section.

# Manual VM Setup

*Time to complete: About 30-90 minutes, depending on your download speed and prior OS knowledge*

## Setting up your OS

The OS that currently works without any problems or further changes is **[Ubuntu 14.04.2 LTS 64-bit](http://old-releases.ubuntu.com/releases/trusty/ubuntu-14.04.2-desktop-amd64.iso)** (the long-term release one). Use anything else at your own risk!

It is also possible to run setup rUNSWift starting with a newer version of Ubuntu, such as Ubuntu 16.04. However, please be aware that this is less well tested, and features of offnao (our support tool) may break. If you wish to do so, **please first follow all the setup instructions below**, then check out [Patching Offnao for newer Ubuntu versions](#patching-offnao-for-newer-ubuntu-versions). It is safer to upgrade from Ubuntu 14.04.2 LTS using:

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get dist-upgrade
```

You can run the OS natively on its own partition, or in a VM, it is recommended to have at least a 30Gb partition and to allocate at least 3GB RAM otherwise `bin/build_setup.sh` will not work.

Make sure you defer installing updates during and after your OS setup.

### Initial packages
If you are installing a brand new machine, then you should need to install:
* git

Additionally, you may find the following packages useful:
* gitk
* vim
* vim-gnome

Finally, it is worth cleaning up space by removing these packages you might not need:
* LibreOffice

Quick commands:

    sudo apt-get install git gitk vim vim-gnome
    sudo apt-get purge libreoffice*
    sudo apt-get autoremove

## Linking GitHub account to your VM

*See also: [[Git workshop]]*

 * [Add two-factor authentication (2FA) to your GitHub Account](https://help.github.com/articles/securing-your-account-with-two-factor-authentication-2fa/) if you haven't already.

    Good 2FA phone apps include [Google Authenticator](https://support.google.com/accounts/answer/1066447?hl=en), [LastPass Authenticator](https://lastpass.com/auth/) and [Microsoft Authenticator](https://play.google.com/store/apps/details?id=com.azure.authenticator&hl=en_US). Make sure your backup codes are in a safe place, like your wallet, or a hotel safe while travelling at competition.

 * [Install your ssh public key into your GitHub account](https://github.com/settings/keys)

    Note: If you don't have one you can make one by running `ssh-keygen -t rsa -b 4096` then hit <kbd>Enter</kbd> three times, the public key will be placed in `~/.ssh/id_rsa.pub` and the private key in `~/.ssh/id_rsa`
 * Set up your git settings to correspond to your GitHub account

    git config --global user.name "Firstname Lastname"

    git config --global user.email "your_email@youremail.com"

## Setting up the build environment

Before beginning, one needs to install a few dependencies such as `CMake` and developer versions of zlib & glib. Try:

    # Should have CMake 2.8.12.2, earlier versions tend to have other issues
    sudo apt-get install cmake zlib1g-dev libglib2.0-dev

    # Extra libraries for 64 bit.
    sudo apt-get install libc6-i386 lib32z1-dev libstdc++6:i386 libbz2-1.0:i386 libfreetype6:i386 libglib2.0-0:i386 libsm6:i386 libxrandr2:i386 libfontconfig1:i386

Get our repository and run the build script by copy and pasting the following:

    git clone git@github.com:UNSWComputing/rUNSWift.git rUNSWift
    cd rUNSWift
    bin/build_setup.sh

**NOTE: You need to be inside the CSE firewall (recommended), or you will need to follow [[ Flashing a Nao ]] (advanced) to get the `LINUX_CTC_ZIP` file originally from Aldebaran into your `rUNSWift/ctc` directory.**

This will:
 * Set up your .bashrc with some robocup environment variables.
 * Add your ssh key to the image on the robots.
 * Download the cross toolchain (CTC) and generate some Makefiles.
 * Do an initial release and debug build.

This can take a long time, maybe half an hour, so now is a good time to go do something else.

Once this is finished, **close all open terminals**. The build setup creates some environment variables, which need to be loaded before you can run any of the runswift code (such as offnao).

## Patching Offnao for newer Ubuntu versions
If you are running Ubuntu 16.04, then it is possible to patch offnao to run on these environments.

You must have first followed the instructions for [Manual VM Setup](#manual-vm-setup) above.

You will need to download a patch from the runswift server. 
Quick command - (assuming your working directory is the runswift root directory):
```sh
mv $RUNSWIFT_CHECKOUT_DIR/ctc/sysroot_legacy/usr/lib/libGL.so.1 $RUNSWIFT_CHECKOUT_DIR/ctc/sysroot_legacy/usr/lib/libGL.so.1.backup
wget https://github.com/UNSWComputing/rUNSWift-assets/releases/download/v2017.1/libGL.so.1 --directory-prefix=$RUNSWIFT_CHECKOUT_DIR/ctc/sysroot_legacy/usr/lib
```
Offnao should now be able to run. If you've questions or trouble getting this to work, please contact Gary.

# Post Setup

*Time to complete: As long as you wish - meander and lazily learn or race as fast as you can*

## Pulling the latest code.
If you've used the VM to setup, your code will be behind the latest commit in master. You should rectify this by using `git pull`. Make sure you've followed the instructions in [Setting up a GitHub account](#setting-up-a-github-account) first. You will also wish to rebuild - cd to `build-release`, and type `make -jx`, where x is the number of threads available on your machine/vm (usually 4).

## Editors
You will want to install a text editor to work on the code. An IDE will be useful in this case as the codebase is quite large, and IDE's make it easier to work in these environments.
* CLion is free for university students if you sign up for a free [JetBrains](https://www.jetbrains.com/student/) account.
* [Atom](https://atom.io/) is particularly hackable, e.g. install it then type ``atom .`` in any folder 👍
* Sublime is also another great free text editor you can check out.
* Visual Studio Code (often shortened to `vscode`) is a surprisingly great editor. With some small [configuration](https://code.visualstudio.com/docs/languages/cpp), you can get IntelliSense (autocomplete and more) to work across the entire codebase. It's also easy to build from inside the editor. One useful feature is the ability to jump to class / struct / function definitions. You might also like to [set up a global `.gitignore`](https://help.github.com/articles/ignoring-files/#create-a-global-gitignore) for the `.vscode` files.

## Final Steps

If you have followed the above steps without any issues then go you should be able to run offnao:

    ~/runswift/build-release/utils/offnao/offnao

## Syncing with the robot

You may find [[Networking]] and [[Passwords]] useful here.

Make sure the robot you are syncing with has been set up - check [[Robot Statuses]]. If the robot has not been set up, see [[Flashing a Nao]]. If you are syncing with Tank:

    nao_sync <args> tank

or

    nao_sync <args> -b build-relwithdebinfo tank

The first is for normal execution, the second is if you want debugging symbols (it will sync from the build-relwithdebinfo directory instead, so make sure you run 'make' there instead of in your build-release directory). You should learn more about the other nao_sync flags with `nao_sync --help`. Typically, you will run `nao_sync -rad tank` to sync runswift and remove old files.

## Running the code

You can either:
* ssh into the robot with `ssh nao@tank.local` and run the 'runswift' command, or
* Load runswift by triple tapping the robot's chest or sliding your finger across the touch panels on it's head from front(eyes) to back(fan)

Either way, after doing this you will need to double tap the chest to make it stiffen its body, and then toggle to penalized/playing modes with single taps of the chest.

There is more information on the button press interface and indicator lights at [[Button Presses For Nao]].

## Summary

So basically, from now on your build process will be something like:
 1. \<Make changes to code\>
 1. cd build-release; make;
 1. nao_sync -rad tank
 1. Launch the robot through button presses
 1. (Depending on what you're doing) Connect to the robot via [[Offnao]] and track its progress

Or:

    nao_build -j8 && nao_sync -rad odin.local && ssh nao@odin.local

    # Then type one of the following commands:
    # Start runswift - alternative to button presses
    runswift

    # Then connect in OffNao


## Bonus: Nao command line workflows reference 

Legend
* Items with a `PC$` prefix should be done on your local machine
* Items with a `nao$` prefix should be run at a terminal on the Nao robot.

```
    # Create a relwithdebinfo build on PC + Nao
    PC$ nao_build_gdb
    PC$ nao_sync_gdb
    PC$ ssh nao@odin.local
    nao$ runswift_gdb

    # Start runswift, force choosing player 5 and run the Striker.py behaviour
    nao$ runswift -n5 -s Striker
    
    # Restart libagent
    nao$ nao restart

    # Start libagent with debugging output printed to the console
    nao$ nao stop
    nao$ naoqi -d
```


## Optional setup

The image, and Ubuntu by default, is configured to use the Bash shell. Zsh, an alternative shell, may be useful when combined with the oh-my-zsh plugin, which allows the terminal to display branch information about the rUNSWift folder. One may also wish to install [tig](https://github.com/jonas/tig):

```
sudo apt install tig
```

Get an offline copy of this wiki on your local machine:

    git clone git@github.com:UNSWComputing/rUNSWift.wiki.git

## Ready to share code

Great! Please see [CONTRIBUTING](../blob/master/CONTRIBUTING.md)