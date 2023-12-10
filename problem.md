yay -S auto-cpufreq
systemctl enable --now auto-cpufreq 
Bluetooth
https://www.jeremymorgan.com/tutorials/linux/how-to-bluetooth-arch-linux/

To add a right-click context menu option in Dolphin using the admin:/// protocol, you can create a service menu. Here's a step-by-step guide:
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
