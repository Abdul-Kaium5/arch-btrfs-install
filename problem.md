yay -S auto-cpufreq
systemctl enable --now auto-cpufreq 

Bluetooth

Step 1: Install Bluez and Blueman

The first step is to install some utilities.
I will install:
```
sudo pacman -S bluez
sudo pacman -S bluez-utils
sudo pacman -S blueman
```
Next, make sure the btusb Kernel module is loaded:
```
lsmod | grep btusb
```
![image](https://github.com/ab-kaium/arch-btrfs-install/assets/101384847/50e6c42e-6f16-4f75-ac48-c47c451187e9)
Here’s a trick to help it find adapters, even if you plug one in:

Search for autoenable:
![image](https://github.com/ab-kaium/arch-btrfs-install/assets/101384847/01c5dd22-6a97-4c45-a2f4-4a08aca612cc)
2. Enable the Service

Next, we want to try starting up the service:
```
sudo systemctl start bluetooth.service
```
If you want it to start up automatically, enable it:
```
sudo systemctl enable bluetooth.service
```
Now we should be up and running. You can turn on all adapters with Blueman:

 a right-click context menu option in Dolphin using the admin:/// protocol, you can create a service menu. Here's a step-by-step guide:
1. Create a Service Menu:

    Navigate to the ServiceMenu directory:

    Open your terminal and create the necessary directories if they don't exist:

    bash

mkdir -p ~/.local/share/kservices5/ServiceMenus/

Create a .desktop file for your service menu:

Use your preferred text editor to create a .desktop file. For instance:

bash

nano ~/.local/share/kservices5/ServiceMenus/admin_dolphin.desktop

Write the content for the service menu file:

Add the following content to the .desktop file:

plaintext

    [Desktop Entry]
    Type=Service
    X-KDE-ServiceTypes=KonqPopupMenu/Plugin
    MimeType=inode/directory;
    Actions=openAsAdmin;

    [Desktop Action openAsAdmin]
    Name=Open Dolphin as Administrator
    Icon=system-run
    Exec=dolphin admin://"%f"

    Save and close the file:

    Save the changes and close the text editor.

    Restart Dolphin:

    Close any open instances of Dolphin and restart it to apply the changes. You can either log out and log back in or restart your system, or simply kill the Dolphin process and reopen it


Run Thunar as Root: In the terminal, enter the following command and press Enter:

bash

pkexec thunar

This command will prompt you to enter your password. Upon successful authentication, Thunar will open with root privileges, allowing you to perform file management tasks that require administrative access.


https://wiki.archlinux.org/title/Mkinitcpio#Possibly_missing_firmware_for_module_XXXX

Install Firmware Packages:
You can use yay or any AUR helper to download and install the required firmware packages. For instance, assuming you're using yay:

bash

yay -S aic94xx-firmware ast-firmware linux-firmware-qlogic linux-firmware-bnx2x linux-firmware-liquidio linux-firmware-mellanox linux-firmware-nfp wd719x-firmware upd72020x-fw

Replace the firmware package names with the correct ones you've identified for the missing modules.

Rebuild Initramfs:
After installing the firmware packages, rebuild the initramfs for your Linux kernel:

bash

sudo mkinitcpio -p linux

This command will regenerate the initramfs with the newly installed firmware.

Reboot:
Once the initramfs is rebuilt without warnings, reboot your system to verify that the Plymouth theme or any changes you made to the boot process are working as intended.






    sudo fdisk -l

You should get a long return that includes something like this:

Device             Start        End   Sectors   Size Type
/dev/nvme0n1p1      2048    1050623   1048576   512M EFI System
/dev/nvme0n1p2   1050624  874729471 873678848 416.6G Linux filesystem
/dev/nvme0n1p3 874729472  874762239     32768    16M Microsoft reserved
/dev/nvme0n1p4 874762240 1000214527 125452288  59.8G Microsoft basic data

    Get the UUID of the EFI partition sudo blkid /dev/nvme0n1p1 (replace nvme0n1p1 with the correct partition for you)

Return: dev/nvme0n1p1: UUID="3C26-6A4C" BLOCK_SIZE="512" TYPE="vfat" PARTLABEL="EFI System Partition" PARTUUID="3b64b43f-e7eb-4ac8-a32c-9af2edf64d0d"

    Grant yourself write permission to the '40_custom' file in /etc/grub.d

    Open the terminal (ctrl+alt+t) and run the following commands:
    cd /etc/grub.d
    sudo chmod o+w 40_custom

    Open the 40_custom file
    open ./40_custom

    Write the following at the bottom of the file and replace 3C26-6A4C with the correct UUID:

menuentry 'Windows 11' {
    search --fs-uuid --no-floppy --set=root 3C26-6A4C
    chainloader (${root})/EFI/Microsoft/Boot/bootmgfw.efi
}

    Save the file and close the editor.

    Back in the terminal, remove write permissions.
    sudo chmod o-w 40_custom

    Update GRUB using sudo update-grub

    (Optional) You can confirm that your change was successful by going to /boot/grub/grub.cfg and checking lines 243-251. It should reflect your edits in the 40_custom file

    Reboot your computer reboot




Creating snapshots
```shell
$ btrfs subvolume snapshot -r / /.snapshots/@home-`date +%F-%R`
```
To check the dump (/ . -p)
```shell
$ btrfs subvolume list -p .
```
Note that each subsection has its own ID number.
```shell
$ cd ../..
$ rm -rf *
```
And recover from the snapshot
```shell
$ mount /dev/sda1 /mnt
$ btrfs subvolume delete /mnt/@home
$ brtfs subvolume snapshot /mnt/@snapshots/@home-2017-05-16-20:19 /mnt/@home
```
Restart the machine 20.5500

The function of creating snapshots in BTRFS is implemented quite accurately, and its use does not present any difficulties.



 Step 1. Installation.

The process of using Zram is the same on all Linux distributions, in that you install the Zram package and then enable it to run at boot.

Arch Based Distributions.

For example, on Arch based distributions, you install the zram-generator package with the following Terminal command:

sudo pacman –S zram-generator   

By default, Zram is configured to use 50% of available RAM, but you can modify this behaviour, by using a Zram configuration file.

By default, this will not exist, but can be created with the following Terminal commands.

First change location to the systemd directory with the below command:

cd /etc/systemd/   

And then:

sudo nano zram-generator.conf   

To create and open the conf file.

Within the file add the following:

[zram0]   
Zram-size =ram / 2   
EOF   

In this case, zram-size =ram / 2 refers to using 50% of your system resources.

So, make any changes, save the file, and then reboot the machine to complete the process. 



**for intall arch btrfs with archinstall use premounted directory[/mnt] in disk section other setting as usual you like.before that create btrfs subvolume and the just run archinstaller.**
