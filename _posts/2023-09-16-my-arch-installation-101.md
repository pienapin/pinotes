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
---
I like to reinstall my operating system. Some people would say that I am weird for this. But I just love to have a clean operating system after I used it and made a chaos out of it.

By chaos, it is different in each of my system. For your information, I use Windows on my Desktop PC and Arch Linux on my Laptop and Virtual Machine (Desktop PC).

In Windows, the common problems are red storage space (almost full) and unused files/apps/packages. I can delete them, but when it is too much, I think it is better to just reinstall the whole system.

In Arch Linux, the common problems are f* up configs and unused files/apps/packages. I can fix and delete them, but when I don't know anymore what the hell am i doing, again it is better to just reinstall the whole system.

The reinstall process of Windows is easy, press next, next, and next, and you are done. But, Arch Linux is a different breed, you should do every step, such as setting up the partition, formatting the partition, installing the packages, and many more, manually.

It is easy now with the existing installation script "archinstall", but it is not as cool and as challenging as the manual way. I also think that installing Arch Linux in manual way is a practice of mindfulness, so it gives positive impact in your mental health, unless you messes up the installation i.e. deleting wrong partition.

Therefore, I always install Arch Linux in manual way. It is not that hard too, you just have to follow the instruction in Arch Wiki. But after installing Arch Linux a lot of times, I have my own preferred packages and configs to install. Thus, it is a little bit different with the guide on the Wiki.

So i made this guide, for myself and anyone that is interested in installing Arch Linux my way.

## The Could Have (Alternative Ways)
---
Instead of reinstalling, I could have just took snapshot of my clean Arch Linux installation, but I haven't tried snapshotting yet. I will though, maybe.

I could have wrote script for Arch Linux installation my way too, well, I will honestly.

However, this guide will be helpful if i ever want to just do it the manual way or anyone that is installing it for the first time.

## The Requirements
---
* Laptop / Desktop PC
  * at least 512 MiB RAM
  * at least 40 GiB Storage
  * with working internet connection
* Arch Linux Installation ISO
* USB Drive (at least 1 GiB)

That's it.

## The Brief Steps
---
1. Prepare the USB drive with Arch Linux ISO in it.
2. Boot to the ISO
3. (Optional) Set font to make it easier to see
```bash
setfont ter-132b
```
4. Make sure your connection working by ping to archlinux.org (Ethernet should work by default / Use iwctl for Wi-Fi)
5. Partition your disk
```bash
fdisk -l # to see what devices are available, recognize one you want to use
fdisk /dev/sda # could be /dev/sdb or /dev/sdc depends on your device
```
In fdisk, more or less, it will be like this
```bash
m # to see commands
n # make new partition
p # chcose primary partition type
1 # partiton number (This one is for boot partition)
# just enter
+300M # Size of partition (up to you, 300MiB is minimum for me)
t # change partition type
ef # choose "EFI" as the type of the partition
n # make new partition for root
p
2
+60G # again size of partition, up to you, it is for your main storage space
p # this one is for printing your partition table, to see what have you done
# it is not saved yet, so if you want to cancel it, you can just quit with 'q'
w # this is to write the partition table that we have made (make sure you have done the partition right)
```
6. Format the partition
```bash
mkfs.fat -F 32 /dev/sda1
mkfs.ext4 /dev/sda2
```
7. Mount the partition
```bash
mount /dev/sda2 /mnt
mount --mkdir /dev/sda1 /mnt/boot
```
8. Install the essential packages
```bash
pacstrap -K /mnt base base-devel linux-lts linux-lts-headers linux-firmware intel-ucode neovim git networkmanager man
```
9. Generate an fstab file
```bash
genfstab -U /mnt >> /mnt/etc/fstab
```
10. Chroot into the new system
```bash
arch-chroot /mnt
```
11. Set the timezone
```bash
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```
12. Localization
```bash
nvim /etc/locale.gen # in neovim, uncomment UTF-8 en, kor, ja, and needed locales
locale-gen
echo "LANG=en_US.UTF-8" >> /etc/locale.conf
```
13. Network Configuration
```bash
echo 119 >> /etc/hostname #  echo whatever you want the hostname to be, in my case, 119
systemctl enable NetworkManager.service
```
14. Set user and password
```bash
passwd # change password for root
useradd -mG wheel,video pien # the 'pien' is username, you can change it to whatever you want
passwd pien # change password for 'pien' user
EDITOR=nvim visudo # uncomment the line below "Uncomment to allow mebers of group wheel to execute any command"
```
15. Install bootloader (I use rEFInd)
```bash
pacman -S refind
refind-install --usedefault /dev/sda1 # this will install refind to /dev/sda1
mkrlconf # this will generate config file
nvim /boot/refind_linux.conf # remove the first two line (just left out the boot with minimal option)
```
16. Exit the chroot and reboot
```
exit
umount -a
reboot
```
17. Congratulations, Arch Linux have been installed on your devices. Login to your system. Every next step below is optional because it is for my common configuration.

18. Install your preferred AUR package manager (I use [paru](https://github.com/Morganamilo/paru))
```bash
sudo pacman -S rustup # rust is needed to build paru
rustup default nightly
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
cd ..
rm -rf paru
sudo nvim /etc/paru.conf # uncomment BottomUp
```
19. Replace sudo with doas
```bash
paru -S opendoas
sudo nvim /etc/doas.conf
# insert line below
# permit persist setenv {PATH=/usr/local/bin:/usr/local/sbin:/usr/bin:/usr/sbin} :wheel
# end with newline
doas ln -s $(which doas) /usr/bin/sudo # symlink sudo to doas
doas chown -c root:root /etc/doas.conf # secure doas.conf file
doas chmod -c 0400 /etc/doas.conf
doas pacman -R base-devel # so we can remove sudo
doas pacman -R sudo # remove sudo
```
20. Install display server (xorg or wayland, I use both) and graphic driver
```bash
paru -S xorg xf86-video-amdgpu # for AMD GPU, xf86-video-intel for Intel GPU
```
21. Install display manager (I use [ly](https://github.com/fairyglade/ly))
```bash
paru -S ly
doas systemctl enable ly.service # reboot to test if you want, then login to shell
```
22. Install your preferred graphical environments (I use [hyprland](https://github.com/hyprwm/Hyprland)), other essential packages for graphical environment, such as terminal (I use alacritty), browser (I use Google Chrome Dev), app launcher (I use wofi), file manager (I use dolphin), etc
```bash
paru -S hyprland alacritty google-chrome-dev wofi vlc dolphin ffmpegthumbs kdegraphics-thumbnailers breeze-icons p7zip-gui pulseaudio pavucontrol
mkdir -p .config/hypr # while in home (~)
mkdir .config/alacritty
cp /usr/share/hyprland/hyprland.conf .config/hypr/
cp /usr/share/doc/alacritty/example/alactritty.yml .config/alacritty/
nvim .config/hypr/hyprland.conf # change kitty to alacritty; change key that is binded to alacritty to RETURN; change key that is binded to killactive to Q; change key that binded to wofi to SPACR
```
23. Alternatively, install awesomewm
```bash
paru -S awesome rofi
mkdir -p .config/awesome # while in home (~)
cp /etc/xdg/awesome/rc.lua ~/.config/awesome/
nvim .config/awesome/rc.lua # change terminal to alacritty and add key bind to use rofi as app launcher with mod+space
# awful.key({ modkey,     }, "space" , function () awful.spawn('rofi -show drun') end, {description = "open app launcher", group = "launcher" }),
```