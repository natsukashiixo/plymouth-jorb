# plymouth-jorb

adds the jorb to your linux boot
![thejorb](https://github.com/user-attachments/assets/73bfe2b1-c683-40c5-9680-5b251017b1d4)

shamelessly borrowed from https://github.com/adi1090x/plymouth-themes

## how to install for people who kinda know what they're doing
clone repo + `cp plymouth-jorb/jorb /usr/share/plymouth/themes/`</br>
then `plymouth-set-default-theme -R jorb`

## how to install yap version

reading comprehension check: `...` here is used to denote that there might be something or nothing where the dots appear and that parameters can safely be placed between whatever that may be.

first make sure you have plymouth installed: `which plymouth`
if not then install it from your package manager

add `quiet splash` to your kernel parameters if you haven't already.
for systemd-boot:
```sudo nano /boot/loader/entries/yourkernelhere.conf```
using my kernel as an example:
```
# Created by: name
# Created on: 2025-06-28_12-39-00
title   Arch Linux (bazzite)
linux   /vmlinuz-linux-bazzite
initrd  /initramfs-linux-bazzite.img
options ... rw quiet splash ...
```
i.e. put it after rw

if using another bootloader please reference the [arch wiki page](https://wiki.archlinux.org/title/Kernel_parameters)

add plymouth to your initramfs.</br>
for mkinitcpio:
`sudo nano /etc/mkinitcpio.conf`
`HOOKS=(... plymouth ...)`

for dracut:
`sudo nano /etc/dracut.conf.d/myflags.conf`
`add_dracutmodules+=" plymouth "`

reference page [here](https://wiki.archlinux.org/title/Plymouth)

clone repo
`git clone https://github.com/natsukashiixo/plymouth-jorb`
copy theme to plymouth theme folder, might need root. path in example is for arch linux
`cp plymouth-jorb/jorb /usr/share/plymouth/themes/`
set it as default theme
`plymouth-set-default-theme -R jorb`
-R here rebuilds initramfs, can be skipped if you want to rebuild them on your own later, for instance if you need to add nvidia stuff in initramfs.

## things you might want to add
systems boot pretty fast nowadays so you might want to disable the initial delay in `/etc/plymouth/plymouthd.conf`
`sudo nano /etc/plymouth/plymouthd.conf`
```
[Daemon]
Theme=jorb
ShowDelay=0
```
the jorb is also pretty tiny so you might want to increase the DeviceScale
```
[Daemon]
...
DeviceScale=2
...
```

if you're on **nvidia** you might need to add nvidia stuff to your `mkinitcpio`</br>
fair warning, when I did this it reassigned the names of all of my monitor ports + got weird issues with applications not knowing where they're supposed to start under hyprland. I have yet to figure out what happened and how to fully fix it.</br> 
Your mileage may vary, the journey is your own, good luck.
`sudo nano /etc/mkinitcpio.conf`
```
...
MODULES=(... nvidia nvidia_modeset nvidia_uvm nvidia_drm ...)
...
```
then `sudo mkinitcpio -P` to regenerate initramfs

happy jorbing
