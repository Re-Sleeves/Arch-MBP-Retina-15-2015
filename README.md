# Installing Arch Linux on a 2015 Macbook Pro
Instructions to setup and running for daily use

Items needed for install:

   + Internet connection
   + USB flash drive

## Instructions

1. On a seperate computer download the [Arch Linux iso](https://www.archlinux.org/download/).
2. Use [rufus](https://rufus.ie/en/) to burn the iso to the USB flash drive.
3. Boot to USB drive.

   + Power down your Macbook
   + Insert USB
   + Power on, press and hold the `Alt/Option` key
   + On the UEFI selection menu, select EFI/USB boot option
    
4. Connect to wifi.

   + Type in `iwctl`
   + List all wireless devices: `device list`; typically for laptops the wirless device will be `wlan0`
   + List all available networks: `station name get-networks`
   + To connect to the network: `station name connect SSID`
   + After typing in the password wait a few seconds before `exit`
    
5. Initialize the `archinstall`.

   Use the spacebar for toggling selections and enter to proceed.

   + Keep language and locals the same (unless different language)
   + Select United States and Canada for Mirrors
   + For disk configuration select the disk you want to run Arch linux on and select `btrfs` for the file system
     + Use default structure
     + Select enable copy on write
   + Skip to bootloader and select `Systemd-boot`
   + Change or keep the hostname (name of computer)
   + Create a root password and create a user account
     + Typically main user can be selected as `sudo`
   + In profile select your prefered destop enviroment (I prefer KDE)
     + Keep graphics driver `All open-source` and greeter `sddm`
   + Select `pipewire` in audio
   + In kernels I prefer using `linux-lts` for long term support; `linux` works just as fine
   + For additional packages paste the following:
     ```
     facetimehd-firmware facetimehd-dkms flatpak linux-lts-headers noto-fonts noto-fonts-cjk noto-fonts-emoji noto-fonts-extra ttf-ms-win11-auto intel-undervolt macfanctl
     ```
   + Select `NetworkManager` in network configuration if selected Gnome or KDE for the desktop enviroment
   + Select your timezone and keep automatic time sync to `true`
   + Install (don't have to chroot after installation, just reboot)
