---
title: my arch installation 101
author: pienapin
date: 2023-09-16 14:55:00 +0700
categories: [guide]
tags: [arch, linux, technical]
pinned: true
toc: true
---

## The Why
I like to reinstall my operating system. Some people would say that I am weird for this. But I just love to have a clean operating system after I used it and made a chaos out of it.

By chaos, it is different in each of my system. For your information, I use Windows on my Desktop PC and Arch Linux on my Laptop and Virtual Machine (Desktop PC).

In Windows, the common problems are red storage space (almost full) and unused files/apps/packages. I can delete them, but when it is too much, I think it is better to just reinstall the whole system.

In Arch Linux, the common problems are f* up configs and unused files/apps/packages. I can fix and delete them, but when I don't know anymore what the hell am i doing, again it is better to just reinstall the whole system.

The reinstall process of Windows is easy, press next, next, and next, and you are done. But, Arch Linux is a different breed, you should do every step, such as setting up the partition, formatting the partition, installing the packages, and many more, manually.

It is easy now with the existing installation script "archinstall", but it is not as cool and as challenging as the manual way. I also think that installing Arch Linux in manual way is a practice of mindfulness, so it gives positive impact in your mental health, unless you messes up the installation i.e. deleting wrong partition.

Therefore, I always install Arch Linux in manual way. It is not that hard too, you just have to follow the instruction in Arch Wiki. But after installing Arch Linux a lot of times, I have my own preferred packages and configs to install. Thus, it is a little bit different with the guide on the Wiki.

So i made this guide, for myself and anyone that is interested in installing Arch Linux my way.

## The Could Have (Alternative Ways)
Instead of reinstalling, I could have just took snapshot of my clean Arch Linux installation, but I haven't tried snapshotting yet. I will though, maybe.

I could have wrote script for Arch Linux installation my way too, well, I will honestly.

However, this guide will be helpful if i ever want to just do it the manual way or anyone that is installing it for the first time.

## The Requirements
* Laptop / Desktop PC
  * at least 512 MiB RAM
  * at least 40 GiB Storage
  * with working internet connection
* Arch Linux Installation ISO
* USB Drive (at least 1 GiB)

That's it.

## The Things
### 1. Prepare the installation medium
#### - Download the Arch Linux ISO from https://archlinux.org/download/

It might be confusing since there is so much blue text (hyperlink) that you can click, but just scroll down then you will see link for each country, you can choose any or choose one that probably most nearest from your location.

#### - Put the ISO file to your USB drive

If you are from Windows, you can use [Balena Etcher](https://etcher.balena.io/), download it from there, choose between installer or portable (i use the portable one).

![balenaEtcher](/assets/img/posts/balenaEtcher.png)

Open the App, click on "Flash from file" then choose your Arch Linux ISO.

Select your USB drive on "Select target". Then, click "Flash!".

Wait, and you are done!

#### - Boot into your Arch Linux installation medium

Connect your USB drive to your device.

Get in to your system firmware (BIOS setup) by spamming whatever button to open it when booting.

Change your BOOT order so that the USB drive is the first one.

Save and exit.

Congratulations, you are in Arch Linux. But, it is not installed yet, you are only on your USB drive.