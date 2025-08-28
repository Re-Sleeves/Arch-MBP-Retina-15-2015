# Installing Arch Linux on a 2015 Macbook Pro (15")

Arch Linux for 2015 Retina MacBook Pro.

## Requirements

- Wi-Fi  
- USB

## Instructions

### 1. Download the Arch ISO

On a separate computer, download the [Arch ISO](https://www.archlinux.org/download/).

### 2. Create a Bootable USB

Use [Rufus](https://rufus.ie/en/) to burn the ISO to the USB.

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
2. List wireless devices (usually `wlan0`):
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
### 5. Running the Arch Installer

```
archinstall
```

Use the spacebar to select options and `Enter` to continue.

### Recommended Settings

- **Locale & Language:** Keep defaults unless needed.  
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
  - Add a user (assign as `sudo`).
- **Profile:**
  - Select a desktop environment (I prefer KDE).
  - Graphics driver: `All open-source`.
  - Greeter: `sddm`.
- **Audio:** Choose `pipewire`.
- **Kernel:** 
  - Recommended: `linux-lts` (long-term support).
- **Additional Packages:**
  ```
  flatpak linux-lts-headers noto-fonts noto-fonts-cjk noto-fonts-emoji noto-fonts-extra
  ```
- **Network Configuration:**
  - Choose `NetworkManager` (for GNOME or KDE installations).
- **Timezone:** Set yours, and enable automatic time sync.

After setup, proceed with the installation then reboot.

While in systemd, use arrow keys to select the kernel; use the `d` key to select the default kernel.

## 6. Post-Install: Fix Driver Issues

After logging into KDE, the Wi-Fi and camera do not work out of the box.

### Fixing Wi-Fi

1. Edit your bootloader entry:
   ```
   sudo nano /boot/loader/entries/your-boot-entry.conf
   ```
   - If you're unsure of the file name, list entries:
     ```
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

Install [`yay`](https://github.com/Jguer/yay)

`yay` downloads packages from AUR

Install yay:
```
sudo pacman -S --needed git base-devel && git clone https://aur.archlinux.org/yay.git && cd yay && makepkg -si
```

Install necessary drivers:
```
yay -S facetimehd-firmware facetimehd-dkms
```

Enable the camera module:
```
sudo modprobe facetimehd
```

## Finished

You've successfully installed Arch on a 2015 MacBook Pro.

## Optional Steps

Microsoft Windows 11 Fonts
```
yay -S ttf-ms-win11-auto
```
### Undervolt configuration
1. Download `intel-undervolt`
```
yay -S intel-undervolt
```
2. Load the undervolt configuration
```
sudo nano /etc/intel-undervolt.conf
```
3. Set configuration values:
```
enable yes

undervolt 0 'CPU' -75
undervolt 1 'GPU' -50
undervolt 2 'CPU Cache' -75
undervolt 3 'System Agent' 0
undervolt 4 'Analog I/O' 0
```
4. Apply changes
```
sudo intel-undervolt apply
```
5. Enable undervolting on boot
```
sudo systemctl enable --now intel-undervolt
```
### Increasing fan speed
1. Download `macfanctld`
```
yay -S macfanctld
```
2. Load fan control configuration
```
sudo nano /etc/macfanctl.conf
```
3. Set configuration values:
```
fan_min: 3000
temp_avg_floor: 45
temp_avg_ceiling: 55
temp_TC0P_floor: 50
temp_TC0P_ceiling: 58
temp_TG0P_floor: 50
temp_TG0P_ceiling: 58
```
4. Enable fac control on boot
```
sudo systemctl enable --now macfanctld
```
