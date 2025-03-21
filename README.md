# Installing Arch Linux on a 2015 Macbook Pro (15")

Set up Arch Linux for daily use on a 2015 Retina MacBook Pro.

## Requirements

- Internet connection  
- USB flash drive

## Instructions

### 1. Download the Arch Linux ISO

On a separate computer, download the [Arch Linux ISO](https://www.archlinux.org/download/).

### 2. Create a Bootable USB

Use [Rufus](https://rufus.ie/en/) (on Windows) to burn the ISO to the USB flash drive.

### 3. Boot from USB

1. Power down your MacBook.  
2. Insert the USB drive.  
3. Power on and immediately hold the `Option` (Alt) key.  
4. In the UEFI boot menu, select the `EFI/USB` boot option.
    
### 4. Connect to Wi-Fi

1. Enter the `iwctl` utility:
   ```
   iwctl
   ```
2. List wireless devices (usually `wlan0` on laptops):
   ```
   device list
   ```
3. Show available networks:
   ```
   station wlan0 get-networks
   ```
4. Connect to your Wi-Fi network:
   ```
   station wlan0 connect <SSID>
   ```
5. Wait a few seconds after entering the password, then type:
   ```
   exit
   ```
### 5. Run the Arch Installer

Start the guided installer:
```
archinstall
```

Use the spacebar to select options and `Enter` to continue.

### Recommended Installer Settings

- **Locale & Language:** Keep defaults unless needed otherwise.  
- **Mirrors:** Select United States and/or Canada.  
- **Disk Configuration:**
  - Choose the target disk.
  - Select the `btrfs` file system.
  - Use the default structure.
  - Enable Copy-on-Write (CoW).
- **Bootloader:** Select `systemd-boot`.
- **Hostname:** Set a computer name.
- **User Setup:**
  - Create a root password.
  - Add a user (assign as `sudo` if desired).
- **Profile:**
  - Select a desktop environment (I prefer KDE).
  - Graphics driver: `All open-source`.
  - Greeter: `sddm`.
- **Audio:** Choose `pipewire`.
- **Kernel:** 
  - Recommended: `linux-lts` (long-term support).
  - Alternatively: `linux`.
- **Additional Packages:**
  ```
  flatpak linux-lts-headers noto-fonts noto-fonts-cjk noto-fonts-emoji noto-fonts-extra
  ```
- **Network Configuration:**
  - Choose `NetworkManager` (especially for GNOME or KDE installations).
- **Timezone:** Set yours, and enable automatic time sync.

After setup, proceed with the installation. You **do not need to chroot**—simply reboot when it's done.

## 6. Post-Install: Fix Driver Issues

After logging into KDE, you might notice that Wi-Fi and the camera aren't working.
<!-- add how to install yay -->
Install the necessary drivers:
```
yay -S facetimehd-firmware facetimehd-dkms
```
### Fixing Wi-Fi

1. Edit your bootloader entry:
   ```bash
   sudo nano /boot/loader/entries/your-boot-entry.conf
   ```
   - If you're unsure of the file name, list entries:
     ```bash
     ls /boot/loader/entries/
     ```
     - For `linux-lts`, the file will end with `_linux-lts.conf`.

2. Add the following kernel parameter to the `options` line:
   ```
   brcmfmac.feature_disable=0x82000
   ```
   Example:
   ```
   options root=UUID=xxxx-xxxx ..... brcmfmac.feature_disable=0x82000
   ```
### Fixing the Camera

Enable the camera module with:
```
sudo modprobe facetimehd
```

## 🎉 Done!

You've successfully installed Arch Linux on a 2015 MacBook Pro Retina (15").

## Optional Settings
`ttf-ms-fonts`
What I have noticed with this computer is that it often runs hot with basic use. These next few steps will guide you on undervolting CPU and GPU as well as changing the fan speeds.
`intel-undervolt macfanctl`
