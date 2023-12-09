# arch-install

# mkfs.vfat -F 32 -n EFI /dev/your_drive1
# mkfs.btrfs -L ROOT /dev/your_drive2

iwctl
device list
device wlan0 show
station wlan0 scan
station wlan0 get-networks
station wlan0 connect AB_KAIUM
password
exit
ping google.com
lsblk
cfdisk
pacman -Syy
pacman -S reflector
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
reflector -c "Bangladesh" -f 12 -l 10 -n 12 --save /etc/pacman.d/mirrorlist

2. btrfs subvolumes

We will create few of them to support easy snapshoting with snapper

Mount the root btrfs volume

mount /dev/nvme0n1p6 /mnt

Create subvolume for root, home, var and one for snapshots

btrfs subvolume create /mnt/@root
btrfs subvolume create /mnt/@var
btrfs subvolume create /mnt/@home
btrfs subvolume create /mnt/@snapshots

Mount them.

umount /mnt
mount -o noatime,compress=lzo,subvol=@root /dev/nvme0n1p6 /mnt
mkdir /mnt/{boot,var,home,.snapshots}
mount -o noatime,compress=lzo,subvol=@var /dev/nvme0n1p6 /mnt/var
mount -o noatime,compress=lzo,subvol=@home /dev/nvme0n1p6 /mnt/home
mount -o noatime,compress=lzo,subvol=@snapshots /dev/nvme0n1p6 /mnt/.snapshots

mount /dev/nvme0n1p5 /mnt/boot/efi
pacstrap /mnt base base-devel btrfs-progs efibootmgr;/
genfstab -U /mnt >> /mnt/etc/fstab;/
ln -s /usr/share/zoneinfo/Asia/Dhaka /mnt/etc/localtime
arch-chroot /mnt /bin/bash;/
pacman -S sudo nano git
nano /etc/locale.gen
en_US.UTF-8 UTF-8
bn_BD UTF-8
nano /etc/locale.conf
LC_COLLATE=C
LANG=en_US.UTF-8
LC_TIME=en_US.UTF-8
echo myarch > /etc/hostname
touch /etc/hosts
nano /etc/hosts
127.0.0.1   localhost.localdomain   localhost
::1         localhost.localdomain   localhost
127.0.1.1   localhost.localdomain   your_hostname
nano /etc/sudoers
%sudo ALL=(ALL)
ab-kaium ALL=(ALL) ALL

groupadd sudo
useradd -m ab-kaium
passwd ab-kaium
passwd root(if you want)
useradd -m -g users -G sudo,wheel,power,storage your-username
nano visudo
ab-kaium ALL=(ALL) ALL
pacman -S grub efibootmgr ntfs-3g 
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
pacstrap /mnt base base-devel btrfs-progs efibootmgr (withour  chroot install )

Reboot

Exit from root

exit

Unmount partitions, recursively.

umount -R /mnt
Or,
umount -l /mnt

shutdown now

reboot [then if you want]
Setup Package Manager

After reboot we need to install additional AUR package manager. I prefer yay.

git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si

yay -S pamac-aur

Snapshot time

You can create snapshots like so

btrfs subvolume snapshot -r / /.snapshots/@root-`date +%F-%R`

And to restore from snapshot you just delete the currently used @root and replace it with a earlier snapshot

mount /dev/sda2 /mnt
btrfs subvolume delete /mnt/@root`
brtfs subvolume snapshot /mnt/@snapshots/@root-2015-08-10-20:19 /mnt/@root

and then just reboot :)

you will probably want to use Snapper or something like that to manage your snapshots.


0. Update your system

You might already have used the latest release, but it’s advisable to check for the latest update for your Arch System:

sudo pacman -Syu

1. Installing X server, Desktop Environment and Display Manager

Before installing a desktop environment (DE), you will need to install the X server which is the most popular display server.

sudo pacman -S xorg


Once it’s completed, use any of the below commands to install your favorite desktop environment.

To install GNOME:

sudo pacman -S gnome gnome-extra

To install Cinnamon:

sudo pacman -S cinnamon nemo-fileroller

To install XFCE:

sudo pacman -S xfce4 xfce4-goodies

To install KDE:

sudo pacman -S plasma

To install MATE:

sudo pacman -S mate mate-extra

You will also need a display manager to log in to your desktop environment. For the ease, you can install LXDM.

pacman -S lxdm

Once installed, you can enable to start each time you reboot your system.

systemctl enable lxdm.service

Reboot your system and you will see the LXDM login screen, select your desktop environment from the list and login.

This is how my system looks like with LXDM and GNOME.

Installing KDE Plasma desktop

pacman -S xorg plasma plasma-wayland-session kde-applications 


Once installed, enable the Display Manager and Network Manager services:

systemctl enable sddm.service
systemctl enable NetworkManager.service


Installing Codecs and plugins

Of course, you are going to use your personal system for recreational works like watching videos and listening to your favorite song. But before that, you will have to install codecs for these audio and video files.

Type the below command in the terminal:

sudo pacman -S a52dec faac faad2 flac jasper lame libdca libdv libmad libmpeg2 libtheora libvorbis libxv wavpack x264 xvidcore gstreamer0.10-plugins

However, installing a media player like VLC imports all the necessary codecs and installs it.

sudo pacman -S vlc
sudo pacman -S p7zip p7zip-plugins unrar tar rsync aria2
