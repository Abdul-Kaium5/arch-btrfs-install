# Arch Linux Installation Guide

This guide provides step-by-step instructions for installing **ARCH** Linux and configuring various system settings, including setting up different desktop environments, Btrfs subvolumes, user management, and more.

## Installation Steps

## ⓘ Setup

- **First Create Drive**
```sh
lsblk
cfdisk
```

- **Formatting Drives**

```sh
mkfs.vfat -F 32 -n EFI /dev/nvme0n1p5
mkfs.btrfs -L ROOT /dev/nvme0n1p6
```

- **Connecting to Network**

```sh
iwctl
device list
device wlan0 show
station wlan0 scan
station wlan0 get-networks
station wlan0 connect AB_KAIUM
password
ping google.com
```

## ⓘ  btrfs subvolumes

We will create few of them to support easy snapshoting with snapper.

- **Mount the root btrfs volume**
```sh
mount /dev/nvme0n1p6 /mnt
```
- **Create subvolume for root, home, var and one for snapshots**

```sh
btrfs subvolume create /mnt/@root
btrfs subvolume create /mnt/@var
btrfs subvolume create /mnt/@home
btrfs subvolume create /mnt/@snapshots
```

- **unount**
```sh
umount /mnt
```

- **Mount them.**

```sh
mount -o noatime,compress=lzo,subvol=@root /dev/nvme0n1p6 /mnt
mkdir /mnt/{boot,var,home,.snapshots}
mount -o noatime,compress=lzo,subvol=@var /dev/nvme0n1p6 /mnt/var
mount -o noatime,compress=lzo,subvol=@home /dev/nvme0n1p6 /mnt/home
mount -o noatime,compress=lzo,subvol=@snapshots /dev/nvme0n1p6 /mnt/.snapshots
mount /dev/nvme0n1p5 /mnt/boot/efi
```
- **Arch Chroot**

```sh
pacstrap /mnt base base-devel btrfs-progs efibootmgr grub ntfs-3g
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt /bin/bash
```

- **Set Mirror**

```sh
pacman -Syy
pacman -S reflector
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
reflector -c "Bangladesh" -f 12 -l 10 -n 12 --save /etc/pacman.d/mirrorlist
```

## ⓘ  Arch tweaks

- **Update your system**
You might already have used the latest release, but it’s advisable to check for the latest update for your Arch System:
```sh
pacman -Syu
```

- **Installing X server, Desktop Environment and Display Manager**
Before installing a desktop environment (DE), you will need to install the X server which is the most popular display server.

```sh
pacman -S xorg
```
Once it’s completed, use any of the below commands to install your favorite desktop environment.

#### To install GNOME:
```sh
pacman -S gnome gnome-extra
```
#### To install KDE:
```sh
pacman -S plasma plasma-wayland-session kde-applications
```
Once installed KDE, enable the Display Manager and Network Manager services:
```sh
systemctl enable sddm.service
systemctl enable NetworkManager.service
```

- **Installing Codecs and plugins**

Of course, you are going to use your personal system for recreational works like watching videos and listening to your favorite song. But before that, you will have to install codecs for these audio and video files.However, installing a media player like VLC imports all the necessary codecs and installs it.

```sh
pacman -S a52dec faac faad2 flac jasper lame libdca libdv libmad libmpeg2 libtheora libvorbis libxv wavpack x264 xvidcore gstreamer0.10-plugins
```

- **Arch Package**

```sh
pacman -S sudo nano git sudo vlc networkmanager
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```

- **Locale Generate**

```sh
ln -s /usr/share/zoneinfo/Asia/Dhaka /mnt/etc/localtime
nano /etc/locale.gen
en_US.UTF-8 UTF-8
bn_BD UTF-8
nano /etc/locale.conf
LC_COLLATE=C
LANG=en_US.UTF-8
LC_TIME=en_US.UTF-8
```

- **User**

```sh
groupadd sudo
useradd -m ab-kaium
passwd ab-kaium
passwd root(if you want)
useradd -m -g users -G sudo,wheel,power,storage,video,audio your-username
nano visudo
your-username ALL=(ALL) ALL
nano /etc/sudoers
%sudo ALL=(ALL)
your-username ALL=(ALL) ALL
echo your-username > /etc/hostname
touch /etc/hosts
nano /etc/hosts
127.0.0.1   localhost.localdomain   localhost
::1         localhost.localdomain   localhost
127.0.1.1   localhost.localdomain   your_hostname
```

- **Setup Package Manager**

After reboot we need to install additional AUR package manager. I prefer yay.

```sh
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
yay -S pamac-aur
```

- **Snapshot time**

You can create snapshots like so

```sh
btrfs subvolume snapshot -r / /.snapshots/@root-`date +%F-%R`
```

And to restore from snapshot you just delete the currently used @root and replace it with a earlier snapshot

```sh
mount /dev/nvme0n1p6 /mnt
btrfs subvolume delete /mnt/@root`
brtfs subvolume snapshot /mnt/@snapshots/@root-2015-08-10-20:19 /mnt/@root
```

and then just reboot :)

you will probably want to use Snapper or something like that to manage your snapshots.



- **Reboot**

```sh
exit
umount -R /mnt Or, umount -l /mnt
shutdown now
reboot [then if you want]
```


