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
yay -S kio-admin

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
Restart Dolphin
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






Hey man, I've been there when I started too.

    Make sure you've installed ntfs-3g $ sudo pacman -S ntfs-3g.

    Make sure you've installed os-prober $ sudo pacman -S os-prober.

    Edit grub to use os-prober $ sudo nano /etc/default/grub Find the last (or towards the bottom) line and make it say:. GRUB_DISABLE_OS_PROBER=false. Save and exit. Ctrl o. Ctrl x.

    Make sure you've mounted windows $ sudo mount -t ntfs /dev/nvme**** /mint/windows. (Put whatever partition windows is on where the stars are).

    Make sure you've installed grub to the correct drive (pretty sure you have or it wouldn't boot Linux). $ sudo grub-install /dev/sd*

    Re run grub config. $ sudo grub-mkconfig -o /boot/grub/grub.cfg
    (Make sure you fix that last to go to the correct location in case your grub.cfg is it n a different place. ).

Hope that gets you there.


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


Open the configuration file in a text editor using elevated privileges (such as using sudo or su):

bash

sudo nano /etc/sddm.conf

Configure Num Lock:

Add or modify the following line in the [General] section of the SDDM configuration file to set Num Lock at login:

plaintext

Numlock=on

Save the changes made to the file and exit the text editor.

Restart SDDM:

After making changes to the configuration file, restart the SDDM service to apply the new settings:

bash

sudo systemctl restart sddm


**for intall arch btrfs with archinstall use premounted directory[/mnt] in disk section other setting as usual you like.before that create btrfs subvolume and the just run archinstaller.**


if thuner image thumbnail not showing up
<code>yay -S tumbler gdk-pixbuf2</code>





In order to remove that message, go into /boot/grub/grub.cfg. Scroll down until you see the line ### BEGIN /etc/grub.d/10_linux ###. Right below, you'll see something like this:
```
menuentry 'Arch Linux' --class arch --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-simple-cdb0b113-f657-4b1b-a8e9-3fd0fb2c55d2' {
    load_video
    set gfxpayload=keep
    insmod gzio
    insmod part_gpt
    insmod fat
    set root='hd1,gpt1'
    if [ x$feature_platform_search_hint = xy ]; then
      search --no-floppy --fs-uuid --set=root --hint-bios=hd1,gpt1 --hint-efi=hd1,gpt1 --hint-baremetal=ahci1,gpt1  AAF7-73DC
    else
      search --no-floppy --fs-uuid --set=root AAF7-73DC
    fi
    echo    'Loading Linux linux-selinux ...'
    linux   /vmlinuz-linux-selinux root=UUID=cdb0b113-f657-4b1b-a8e9-3fd0fb2c55d2 rw cryptdevice=/dev/sdb3:root security=selinux selinux=1 init=/usr/bin/e4rat-lite-preload
    echo    'Loading initial ramdisk ...'
    initrd  /intel-ucode.img /initramfs-linux-selinux.img
}
```
Remove the lines that begin with echo and that boot message will be gone. Add quiet to your kernel parameters to disable boot messages in the kernel.

plymouth-theme-arch-breeze-git

```sudo updatedb```
for font set for all
``` sudo lxappearance```

if pamac aur icon not showing -need to reinstall
6

If you just want to disable the notifications, this will do the trick:

$ xfconf-query --channel xfce4-power-manager --property /xfce4-power-manager/general-notification --set false
Note: this will disable all notifications, including low battery warnings.

You can also do this with the GUI by toggling the "Show notifications" checkbox under "Appearance".

setup howdy for time save
change sddm font
```FILES
       /usr/lib/sddm/sddm.conf.d
              System configuration directory

       /etc/sddm.conf.d
              Local configuration directory

       /etc/sddm.conf
              Local configuration file for compatibility
```
``
