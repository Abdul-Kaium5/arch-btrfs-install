# Arch Linux Installation Guide
  **Have you ever heard someone say, <code> Oh – by the way, I use Arch Linux! </code>**
> [!Caution]
> # BTW, I use Arch Linux!

> [!Tip]
 > - Linux: an OS or a kernel? For me, it's freedom. Arch Linux: freedom to craft an OS. Installing it? A Linux proficiency badge.
 > - Arch Linux: for pros. Complete system control. Choose packages, kernels, desktops. Your call. 
 > - This is because installing Arch Linux on a machine requires you to have proper knowledge of how different parts of a Linux distribution work. So running Arch Linux on your system is kind of a testament to your understanding of Linux.

#### Step-by-step guide for ARCH Linux setup: system settings, including setting up different desktop environments, Btrfs subvolumes, user management, and more.

 ### ✦ Installation Steps
 
### ⓘ Arch ISO
> [!Important]
>- **Download the Arch Linux ISO**
>  - Grab the ISO from the official website.
>
>     [Click here to download Arch Linux ISO](https://archlinux.org/download/)
>
- **How To Install Arch Linux**
> [!Important]
>  - ### Booting from USB Drive
>
>1. **Ensure a bootable USB drive.**
>   
>2. **Boot from USB:**
>    - The method varies by computer.
>    - Example: On my machine, pressing `F2 key` during boot brings up the bootable devices list.
>    - Find the appropriate method for your computer.
>
>3. **Using Ventoy (Optional):**
>    - Ventoy offers simple multi-OS support.
>
>4. **Select USB Drive:**
>   - Once on the bootable devices list, choose your USB drive to boot.
>   
>5. **Select First Option:**
>    - Choose the first entry from the list.
>###### Arch Linux Installation: Manual Configuration - No GUI; Manual setup required for each aspect; Time and effort needed, but enjoyable with understanding.
![image](https://github.com/ab-kaium/arch-install/assets/101384847/68f9614a-294e-4a8c-af96-d3d6864da9f5)
![image](https://github.com/ab-kaium/arch-install/assets/101384847/2e4cd7ef-edda-45f4-bfcf-a2840a567a78)
- **How To Verify the Boot Mode**
> [!Important]
>```sh
>ls /sys/firmware/efi/efivars
>```
> ###### UEFI Mode Check: Displayed files indicate UEFI mode; Confirm proper mode before proceeding.
![image](https://github.com/ab-kaium/arch-install/assets/101384847/21575574-5afd-4ae6-9f60-5048ae18b76f)
✦ **ⓘ Disk Partitioning**
> [!Caution]
>- **Start with Awareness:**
>  - Proceed with caution; partitioning mistakes may result in data loss. Review the entire section before proceeding.
>  - Replace `/dev/sda` with your device. This command shows existing partitions.

> [!Important]
>- **Identify Connected Disks:**
>  - Use `fdisk` to list available devices and their partitions:
>    ```sh
>    fdisk -l
>    fdisk /dev/sda -l
>    ```
![image](https://github.com/ab-kaium/arch-install/assets/101384847/71bd1c29-6a98-4e0d-b9f1-b25e8f430160)
![image](https://github.com/ab-kaium/arch-install/assets/101384847/eceb3e78-3c4d-4531-825f-ff9f7d560bc6)

- **Partition Table Check:**
> [!Important]
>  - Check existing partitions within the selected device:
>    ```sh
>    fdisk /dev/sda -l
>    ```
>- **cfdisk Option:**
>  - `cfdisk` offers a user-friendly interface for partition manipulation:
>    ```sh
>    cfdisk /dev/sda
>    ```
>    Replace `/dev/sda` with your device.
>
>- **UEFI Partition Setup:**
>  - Use `gpt` for UEFI-based systems. Choose partition types accordingly.
>    ```sh
>    cfdisk /dev/sda
>    ```
![image](https://github.com/ab-kaium/arch-install/assets/101384847/2483b4b5-f906-4324-93b6-b11cb09b9693)
![image](https://github.com/ab-kaium/arch-install/assets/101384847/5e2578ab-5901-4b5a-825d-0304ac67ca6b)
 - **Partition Creation:**
> [!Important]
>   - Create the required partitions:
>     - EFI System Partition: Allocate a minimum of 500MB.
>     - ROOT Partition: Allocate desired space, e.g., 10GB or more.
>     - Swap Partition: Allocate remaining space and set as Linux swap.

![image](https://github.com/ab-kaium/arch-install/assets/101384847/0a866c52-6ea1-4885-abf5-952b0e266cfd)
 
 - **Partition Types and Sizes:**
> [!Important]
>   - Assign types and sizes for each partition carefully.
>     - EFI: Type as EFI System.
>     - ROOT: Default Linux filesystem type.
>     - Swap: Set as Linux swap.
     
 ![image](https://github.com/ab-kaium/arch-install/assets/101384847/016204c2-5773-454b-9202-110cf97552b5)
 ![image](https://github.com/ab-kaium/arch-install/assets/101384847/6b78adbe-27d6-490c-8cc8-ec0d4511b4f6)
 ![image](https://github.com/ab-kaium/arch-install/assets/101384847/fce793bd-1b5b-472a-aadc-d1dd025e0f2b)
 ![image](https://github.com/ab-kaium/arch-install/assets/101384847/051f2158-c0b1-475f-9534-30fec9d4999b)
 ![image](https://github.com/ab-kaium/arch-install/assets/101384847/39f2e454-94ef-457a-89d0-765bef4359d2)

 - **Finalizing Partitions:**
> [!Important]
>   - Confirm choices and write changes to the partition table:
>     - Use the [ Write ] action and confirm changes.
>     - Exit the program by selecting [ Quit ].

![image](https://github.com/ab-kaium/arch-install/assets/101384847/99d9b3c6-57c1-46bd-9337-d5d9a25762b2)

> [!Important]
> - **Note for Arch-Windows Dual Boot:**
>   - If dual-booting with Windows, retain the existing EFI system partition.
> - **Additional Information on Swap:**
>   - For alternatives, consider ZRAM setup or swapfile creation if needed.

- **Formatting Drives**
> [!Caution]
> ```sh
> mkfs.fat -F 32 -n EFI /dev/nvme0n1p5
> mkfs.btrfs -f -L ROOT /dev/nvme0n1p6
> ```

- **Connecting to Network**
> [!Important]
> ```sh
> iwctl
> device list
> device wlan0 show
> station wlan0 scan
> station wlan0 get-networks
> station wlan0 connect ABDUL_KAIUM
> station wlan0 connect "ABDUL KAIUM" [if name contain space]
> password [if showing Operation Failled try at least 3 times]
> ping google.com
> ```
 ![image](https://github.com/ab-kaium/arch-install/assets/101384847/2f241a01-8882-43b6-b640-3b12dad53d89)
 ![image](https://github.com/ab-kaium/arch-install/assets/101384847/55e53732-f913-4de8-b448-2bfd14e270ca)

✦ **Btrfs Subvolumes**
> [!Important]
> We will create few of them to support easy snapshoting with snapper.
> 
> - **Mount the root btrfs volume**
> ```sh
> mount /dev/nvme0n1p6 /mnt
> ```
> - **Create subvolume for root, home, var and one for snapshots**
> 
> ```sh
> btrfs subvolume create /mnt/@
> btrfs subvolume create /mnt/@home
> btrfs subvolume create /mnt/@snapshots
> btrfs subvolume create /mnt/@var
> ```
>```
> btrfs subvolume list /mnt
>```
>
> - **unount**
> ```sh
> umount /mnt
> ```
> 
> - **Mount them.**
> 
> ```sh
> mount /dev/nvme0n1p6 -o subvolid=256 /mnt
> mkdir /mnt/{home,.snapshots,var,.btrfsroot,boot}
> mount /dev/nvme0n1p6 -o subvolid=257 /mnt/home
> mount /dev/nvme0n1p6 -o subvolid=258 /mnt/.snapshots
> mount /dev/nvme0n1p6 -o subvolid=259 /mnt/var
> mount /dev/nvme0n1p6 -o subvolid=5 /mnt/.btrfsroot
> mount /dev/nvme0n1p5 /mnt/boot
> ```

✦ **Arch Chroot**
> [!Important]
> - Installing Packages with pacstrap
> 
> Use the `pacstrap` script to install essential packages in the specified root directory `/mnt`:
> 
> - `base`: Minimal package set defining a basic Arch Linux installation.
> - `base-devel`: Group of packages for building software from source.
> - `linux`: Kernel itself.
> - `linux-firmware`: Drivers for common hardware.
> - `sudo`: Allows executing commands as root.
> - `nano`: Enhanced pico editor clone.
> - `ntfs-3g`: NTFS filesystem driver and utilities.
> - `networkmanager`: Facilitates automatic network detection and configuration.
> - `btrfs-progs`: Utilities for Btrfs filesystems.
> - `efibootmgr`: EFI Boot Manager.
> - `grub`:  GRUB is a Bootloader for managing system boot.
> 
> ```sh
> pacstrap /mnt base base-devel btrfs-progs efibootmgr linux linux-firmware sudo nano grub networkmanager snapper snap-pac
> ```
> - **Generating the Fstab File**
> 
> The `fstab` file defines how disk partitions, block devices, or remote file systems should be mounted into the file system. Unlike some other distributions where this file is auto-generated, on Arch Linux, it needs manual configuration.
> 
> To generate the `fstab` file, execute the following command:
> 
> ```sh
> genfstab -U /mnt >> /mnt/etc/fstab
> ```
> 
> - **Logging into the Newly Installed System using Arch-Chroot**
> 
> At present, you're logged into the live environment and not into your newly installed system.
> 
> To proceed with configuring the newly installed system, log into it by executing the following command:
> 
> ```sh
> sudo pacman -S arch-install-scripts
> arch-chroot /mnt
> ```

 - **Set Mirror**
> [!Important]
> 
> ```sh
> pacman -Syy
> pacman -S  --noconfirm --needed reflector
> cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
> reflector --download-timeout 60 --country Bangladesh,Singapore --age 12 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
> ```
> 
- **Configuring the Time Zone**
> [!Important]
> Once you've switched to root, configuring the time zone is the first step. To view all available time zones, execute the following command:
> 
> ```sh
> ls /usr/share/zoneinfo
> ```
 ![image](https://github.com/ab-kaium/arch-install/assets/101384847/604d0d04-af2e-42d8-9ea1-a1e289c2a548)
 ![image](https://github.com/ab-kaium/arch-install/assets/101384847/2943bac1-7f44-485e-b8a6-3d23563e013d)

> [!Important]
> ##### I live in Dhaka, Bangladesh which resides inside the Asia zone. If I list out the content of Asia, I should see Dhaka there:To set Asia/Dhaka as my default time zone, I'll have to make a symbolic link of the file at the /etc/localtime location:
> 
> ```sh
> ln -sf /usr/share/zoneinfo/Asia/Dhaka /etc/localtime
> ``` 
> The ln command is used for creating symbolic links. The -sf options indicate soft and force, respectively.

- **Configuring Localization**
> [!Important]
> To set up language preferences in Arch Linux, follow these steps:
>
>- **Edit Locale Settings**
> 1. Open the `locale.gen` file using the nano text editor:
> ```bash
> nano /etc/locale.gen
> ```
> 2. Locate the list of languages within the file. Uncomment the languages you want to enable. For example, to enable English (en_US.UTF-8) and Bengali (bn_BD UTF-8, bn_IN UTF-8), remove the `#` symbol in front of the desired languages. Save the file by pressing `Ctrl + O` and exit nano with `Ctrl + X`.

- **Generate Locales**
> [!Important]
> Run the following command to generate the locales based on the edited file:
> 
> ```bash
> locale-gen
> ```

- **Configuring Default Language and Console Keymaps**
> [!Important]
> ## Set Default Language
> Open the `/etc/locale.conf` file and append the following line to set English (en_US.UTF-8) as the default language:
> 
> ```bash
> LANG=en_US.UTF-8
> ```

- **Persisting Console Keymaps**
> [!Important]
> If you modified console keymaps during the initial installation, ensure their persistence by adding configurations to `/etc/vconsole.conf`. For example, if you changed the default keymap to `mac-us`, follow these steps:
> 
> 1. Open the `/etc/vconsole.conf` file:
> 
> ```bash
> nano /etc/vconsole.conf
> ```
> 
> 2. Add the line specifying your preferred keymap:
> 
> ```bash
>KEYMAP=$setkmap
> FONT=ter-v18n
> ```
> 
> This ensures that your chosen keymap remains active whenever you use the virtual console, eliminating the need for manual reconfiguration each time.

### ⓘ  Arch tweaks
> [!Important]
> - **Update your system**
> You might already have used the latest release, but it’s advisable to check for the latest update for your Arch System:
> ```sh
> pacman -Syu
> ```

 - **Installing Graphics Drivers**
> [!Important]
>  - Installing graphics drivers on Arch Linux is a simple process. You need to install specific packages based on your graphics processing unit.
> 
> ```sh
> # for nvidia graphics processing unit
> pacman -S --noconfirm --needed nvidia nvidia-utils nvidia-settings
> 
> # for amd discreet and integrated graphics processing unit
> pacman -S  --noconfirm --needed xf86-video-amdgpu mesa mesa-vdpau xf86-video-amdgpu vulkan-radeon libva-mesa-driver
> 
> # for intel integrated graphics processing unit
> pacman -S  --noconfirm --needed xf86-video-intel mesa libva-intel-driver libvdpau-va-gl vulkan-intel libva-intel-driver libva-utils
> ```

- **Installing X server, Desktop Environment and Display Manager**
> [!Important]
> Before installing a desktop environment (DE), you will need to install the X server which is the most popular display server.
> ```sh
> pacman -S  --noconfirm --needed xorg  xorg-xinput  xorg-server xorg-xbacklight xorg-fonts-misc xorg-xfd xorg-xkill xorg-xrandr xorg-xrdb xorg-xset xorg-xev xorg-xmodmap xorg-xwininfo xorg-xsetroot
> ```
> Once it’s completed, use any of the below commands to install your favorite desktop environment.

- **Installing Files/Terminals/Docs/Editors/Fonts Programs....**
> [!Important]
> ```
> pacman -S --noconfirm --needed ranger highlight trash-cli ueberzug thunar thunar-archive-plugin thunar-media-tags-plugin thunar-volman zip p7zip unzip unrar xarchiver xdg-user-dirs xdg-user-dirs-gtk alacritty xfce4-terminal atril geany geany-plugins vim noto-fonts noto-fonts-emoji terminus-font ttf-dejavu
>```

- **Installing GPU/Touchpad/Mouse Drivers ....**
> [!Important]
>```
> pacman -S --noconfirm --needed xf86-video-fbdev xf86-video-vesa xf86-video-intel xf86-video-ati xorg-xinput xf86-input-libinput numlockx
>```

- **Installing System Programs**
> [!Important]
>```
>pacman -S --noconfirm --needed  acpi autoconf automake binutils bison calc fakeroot gcc gparted gtk-engine-murrine gvfs gvfs-mtp gvfs-afc gvfs-gphoto2 gvfs-smb gvfs-google inotify-tools jq make patch pkg-config polkit powertop python2 sshfs udisks2 wmctrl xclip xdotool xmlstarlet yad sndio
>```

- **Installing Utilities/XFCE tools/Hypervisors ....**
> [!Important]
> ```
> pacman -S --noconfirm --needed baobab dialog dunst gpick htop lxappearance meld ncdu nitrogen plank python-pywal rofi maim slop xfce4-power-manager xfce4-settings xsettingsd kvantum qt5ct virtualbox-guest-utils qemu-guest-agent open-vm-tools xf86-input-vmmouse xf86-video-vmware
> ```

  - **To install BSPWM**
> [!Important]
> ```sh
> pacman -S --noconfirm --needed bspwm rofi wmname sxhkd dunst xclip lxappearance nitrogen pavucontrol polkit-gnome lightdm lightdm-gtk-greeter lightdm-gtk-greeter-settings numlockx picom-simpleanime-git polybar xtitle  checkupdates-aur nerd-fonts-jetbrains-mono
> systemctl enable lightdm.service
>

 - **To install GNOME**
> [!Important]
> ```sh
> pacman -S --noconfirm --needed gnome gnome-extra
> ```

 - **To install KDE**
> [!Important]
> ```sh
> pacman -S --noconfirm --needed plasma plasma-wayland-session kde-applications
> ```
> Once installed , enable the Display Manager and Network Manager services:
> ```sh
> systemctl enable sddm.service [For KDE]
> systemctl enable gdm.service [For Gnome]
> systemctl enable NetworkManager.service
> ```

- **Installing Pipeware**
> [!Important]
>```
> pacman -S --noconfirm --needed pipewire pipewire-pulse pipewire-alsa pipewire-jack
>```

- **Installing Zram**
> [!Important]
>```
> pacman -S --noconfirm --needed zramswap
> systemctl enable zramswap
>```

- **Installing PulseAudio**
> [!Important]
>```
>pacman -S --noconfirm --needed pulseaudio
>```

- **Installing Codecs and plugins**
> [!Important]
> Of course, you are going to use your personal system for recreational works like watching videos and listening to your favorite song. But before that, you will have to install codecs for these audio and video files.However, installing a media player like VLC imports all the necessary codecs and installs it.
> ```sh
> pacman -S --noconfirm --needed a52dec faac faad2 flac jasper lame libdca libdv libmad libmpeg2 libtheora libvorbis libxv wavpack x264 xvidcore ffmpeg ffmpegthumbnailer aom libde265 x265 x264  xvidcore libvpx sdl jasper openjpeg2 libwebp webp-pixbuf-loader unarchiver lha lrzip lzip p7zip lbzip2 arj lzop cpio unrar unzip zip unarj xdg-utils 
> ```

- **Installing support for mounting volumes and removable media devices**
> [!Important]
> ```sh
> pacman -S --noconfirm --needed  libmtp gvfs-nfs gvfs gvfs-mtp dosfstools usbutils net-tools xdg-user-dirs gtk-engine-murrine ntfs-3g
> ```

- **Arch Package**
> [!Important]
> ```sh
> pacman -S --noconfirm --needed git vlc gnome-disk-utility
> grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
> grub-mkconfig -o /boot/grub/grub.cfg
> ```

 - **Configuring Network on Linux**
> [!Important]
> If you haven't installed the `networkmanager` package yet, use the following command:
> ```bash
> pacman -S --noconfirm --needed networkmanager
> ```

- **Setting Hostname**
> [!Important]
>  -Add your chosen hostname and put in your-hostname section(e.g., 'archlinux'):
> ```bash
> echo your-hostname > /etc/hostname
> ```
> then,
> ```bash
> touch /etc/hosts
> nano /etc/hosts
> 127.0.0.1        localhost
> ::1              localhost
> 127.0.1.1          your_hostname
> ```
> Enable the NetworkManager service to manage network connections:
> systemctl enable NetworkManager.service

- **User**
> [!Important]
> ```bash
> groupadd sudo
> useradd -m ab-kaium
> passwd ab-kaium
> passwd root(if you want)
> useradd -m -g users -G sudo,wheel,power,storage,video,audio your-username
> nano /etc/sudoers
> %wheel ALL=(ALL:ALL) ALL [uncomment this line]
> your-username ALL=(ALL) ALL
> ```

- **Setup Package Manager**
> [!Important]
> After reboot we need to install additional AUR package manager. I prefer yay.
> ```bash
> git clone https://aur.archlinux.org/yay.git
> cd yay
> makepkg -si
> yay -S pamac-aur
> ```

- **Installing Microcode Updates**
> [!Important]
> Microcode updates provided by processor manufacturers like Intel and AMD are essential for system stability and security. Arch Linux offers official packages for microcode updates that should be installed on your system.
> ```bash
> # for amd processors
> pacman -S --noconfirm --needed amd-ucode
>
> # for intel processors
> pacman -S --noconfirm --needed intel-ucode
> ```


- **Snapshot time**
> [!Caution]
>You can create snapshots like so
>```
>sudo snapper -c root create-config /
>sudo umount /.snapshots/ [if showing .snapshots already exist]
>sudo rm -r /.snapshots/ [if showing .snapshots already exist]
>sudo snapper -c root create-config / [if showing .snapshots already exist]
>mount -a [if showing .snapshots already exist]
>
>cd
>mkdir AUR
>cd AUR
>curl -L -O https://aur.archlinux.org/cgit/aur.git/snapshot/snapper-rollback.tar.gz
>tar -xf snapper-rollback.tar.gz
> rm snapper-rollback.tar.gz
>cd snapper-rollback
>makepkg -sic
>sudo snapper list
>sudo nano /etc/snapper-rollback.conf
>
>mountpoint = /.btrfsroot [add .(dot) before btrfsroot by default here not dot]
>
> you can now rollback by snapper like
> sudo snapper list
> sudo snapper-rollback [which number you want to restore]
>
>Or yay -S timeshift timeshift-autosnap 
> And then just reboot :)
>yay -S btrfs-assistant
>yay -S grub-btrfs [for detecting btrfs snapshots]
>sudo grub-mkconfig -o /boot/grub/grub.cfg
>sudo systemctl enable grub-btrfsd
>sudo systemctl start grub-btrfsd
 you will probably want to use [Snapper](https://wiki.archlinux.org/index.php/Snapper) and restore with <code>btrfs-assistant</code> or something like that to manage your snapshots.


 - **Reboot**
> [!Caution]
> ```sh
> exit
> umount -R /mnt
> reboot
> ```
> **Wait until you see the GRUB menu.**

![image](https://github.com/ab-kaium/arch-install/assets/101384847/bb050946-6d62-46c9-9177-d36decd7f051)
**Choose Arch Linux from the list and wait until the system finishes booting up.**
![image](https://github.com/ab-kaium/arch-install/assets/101384847/893987ca-256f-46cc-a1ca-cdcba4acdf31)
**Log in with your user credentials.**
![image](https://github.com/ab-kaium/arch-install/assets/101384847/a095ae11-1ae1-4cf8-8e47-a97861e9a72f)
**As you can see, I'm currently using Plasma. Now switch to TTY2 press Ctrl + Alt + F2 key combination. You'll see a console login prompt.**
![image](https://github.com/ab-kaium/arch-install/assets/101384847/7a3b9a72-27f7-4d70-9685-957a2116261f)

- **Managing Packages Using Pacman**
> [!Tip]
> Pacman is the package manager for Arch Linux. Here are some commonly used Pacman commands for package management:
>### Installing Packages
>
>- **Install a single package.**
>
>```bash
>sudo pacman -S <package name>
>```
>- **Install multiple packages.**
>
>```bash
>sudo pacman -S <package name> <package name>
>```
>- **Install from a specific repository.**
>
>```bash
>sudo pacman -S <package repository>/<package name>
>```
>- **Remove a package.**
>
>```bash
>sudo pacman -R <package name>
>```
>- **Remove a package and its dependencies.**
>
>```bash
>sudo pacman -Rs <package name>
>```
>- **Remove a package, its configuration, and dependencies.**
>
>```bash
>sudo pacman -Rn <package name>
>```
>- **Remove orphan packages.**
>
>```bash
>sudo pacman -Qdtq | pacman -Rs -
>```
>- **Upgrade all packages.**
>
>```bash
>sudo pacman -Syu
>```
>- **Search for a package in the database.**
>
>```bash
>sudo pacman -Ss <package name>
>```
>- **Check if a package is installed.**
>
>```bash
>sudo pacman -Qs <package name>
>```
>- **This command will generate a file named installed_packages.txt containing a list of all installed packages using Pacman.**
>
>```bash
>pacman -Q > installed_packages.txt
>```
> **These commands will help you install, remove, upgrade, and search for packages effectively using Pacman.**

***Congratulations! You've successfully installed Arch Linux on your system. 🎉***
***Remember, Arch Linux offers unparalleled flexibility and control but requires a good understanding of Linux. Enjoy exploring and customizing your new Arch setup!***

> [!Tip]
> ###  Credits & Final Notes
>
> This guide is a personalized documentation based on **how I installed Arch Linux** on my system, step by step.
> While writing this, I also referred to several helpful resources to better understand and execute the process —
> especially [**The Arch Linux Handbook – Learn Arch Linux for Beginners** by Farhan Hasin Chowdhury](https://www.freecodecamp.org/news/how-to-install-arch-linux/),
> among others I haven't listed here.
>
> I hope this serves as a helpful and practical reference for anyone looking to install Arch Linux in a similar way.
> Enjoy the power and flexibility of Arch!
