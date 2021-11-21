## Download Manjaro
Headless installation will not work with any variants that comes with a pre-installed Desktop Environment.
Download the minimal edition of [Manjaro forRaspberry Pi 4](https://manjaro.org/download/#raspberry-pi-4-minimal)

## Copy img to sd card
1. extract the *.xz file `unxz -d Manjaro-ARM-minimal-rpi4-$YY.$MM.img.xz`
2. Copy to sd card `dd if=Manjaro-ARM-minimal-rpi4-$YY.$MM.img of=$DEVICE status=progress bs=4k conv=noerror,fdatasync oflag=dsync`

## ssh and install
1. Look for the ip of raspberry pi either on router or use `arp-scan --local`
2. login `ssh root@192.168.x.y`
3. Follow onscreen instructions

## Update system
`sudo pacman-mirrors --continent && sudo pacman -Syyu`

## Install LXDE and VNC
`sudo pacman -S xorg-server xorg-server-common xorg-xinit lxde tigervnc`

## Set VNC password
`vncpasswd`

## Check Available sessionns
`ls /usr/share/xsessions`

## Create config file
use any text editor vim/nano/emacs
sudo vim ~/.vnc/config

insert following to the config file, replace $SESSION with one of the sessions fromt the above command
```
session=$SESSION

geometry=1280x720

localhost

dpi=96
```

## Enable user
sudo vim /etc/tigervnc/vncserver.users
add `:2=$USERNAME` with the username of your choice

## Enable server
systemctl enable vncserver@:2

## Reboot
sudo reboot

## ssh with port forward
`ssh pi@192.168.*.* -L 9902:localhost:5902`

connect with any VNC client at localhost:9902