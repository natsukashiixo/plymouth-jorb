# plymouth-jorb

adds the jorb to your linux boot</br>
![thejorb](https://github.com/user-attachments/assets/73bfe2b1-c683-40c5-9680-5b251017b1d4)

shamelessly borrowed from https://github.com/adi1090x/plymouth-themes

## how to install for people who kinda know what they're doing
clone repo + `cp plymouth-jorb/jorb /usr/share/plymouth/themes/`</br>
then `plymouth-set-default-theme -R jorb`</br>

## how to install yap version

reading comprehension check: `...` here is used to denote that there might be something or nothing where the dots appear and that parameters can safely be placed between whatever that may be.

first make sure you have plymouth installed: `which plymouth`</br>
if not then install it from your package manager</br>

add `quiet splash` to your kernel parameters if you haven't already.</br>
for systemd-boot:</br>
```sudo nano /boot/loader/entries/yourkernelhere.conf```
using my kernel as an example:</br>
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

for mkinitcpio:</br>
`sudo nano /etc/mkinitcpio.conf`</br>
`HOOKS=(... plymouth ...)`</br>

for dracut:</br>
`sudo nano /etc/dracut.conf.d/myflags.conf`</br>
`add_dracutmodules+=" plymouth "`</br>

reference page [here](https://wiki.archlinux.org/title/Plymouth)</br>

clone repo</br>
`git clone https://github.com/natsukashiixo/plymouth-jorb`</br>

copy theme to plymouth theme folder, might need root. path in example is for arch linux</br>
`cp plymouth-jorb/jorb /usr/share/plymouth/themes/`</br>

set it as default theme</br>
`plymouth-set-default-theme -R jorb`</br>

-R here rebuilds initramfs, can be skipped if you want to rebuild them on your own later, for instance if you need to add nvidia stuff in initramfs.</br>

## things you might want to add
systems boot pretty fast nowadays so you might want to disable the initial delay in `/etc/plymouth/plymouthd.conf`</br>
`sudo nano /etc/plymouth/plymouthd.conf`</br>
```
[Daemon]
Theme=jorb
ShowDelay=0
```
the jorb is also pretty tiny so you might want to increase the DeviceScale</br>
```
[Daemon]
...
DeviceScale=2
...
```


if you're on **nvidia** you might need to add nvidia stuff to your `mkinitcpio`</br>
fair warning, when I did this it reassigned the names of all of my monitor ports + got weird issues with applications not knowing where they're supposed to start under hyprland. I have yet to figure out what happened and how to fully fix it.</br> 
Your mileage may vary, the journey is your own, good luck.</br>
`sudo nano /etc/mkinitcpio.conf`</br>
```
...
MODULES=(... nvidia nvidia_modeset nvidia_uvm nvidia_drm ...)
...
```
then `sudo mkinitcpio -P` to regenerate initramfs</br>

happy jorbing
