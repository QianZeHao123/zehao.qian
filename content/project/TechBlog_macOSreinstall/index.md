---
title: Best way to reinstall Mac OS
summary: Instructions for installing a Mac OS boot disk on a Mac OS operating system or on a Windows operating system in an intel-chip MacBook.
tags:
  - CS
date: 2024-09-19
# external_link: https://github.com/QianZeHao123/Machine-Learning
---

If you need to reinstall macOS using a USB drive, follow these steps to create a bootable macOS installer and perform a clean installation:

### Preparation:
1. **USB Drive**: Prepare a USB drive with at least 16GB of storage. Backup any data on the drive, as it will be erased during the process.
2. **Download macOS Installer**: Download the macOS version you need from the App Store or from [Apple's website](https://support.apple.com/en-us/HT211683).
3. **Ensure Network Connection**: It's recommended to keep your Mac connected to the internet during this process to verify the installer.

### Step 1 & Step 2: Format USB Drive and Create Bootable macOS Installer

#### For macOS:
1. **Format the USB Drive**:
   1. Insert the USB drive.
   2. Open **Disk Utility** (find it in Launchpad or by searching with Spotlight).
   3. Select the USB drive and click **Erase**.
   4. Set the format to **Mac OS Extended (Journaled)** and the scheme to **GUID Partition Map**.
   5. Click **Erase** to confirm.

2. **Create Bootable Installer**:
   1. Open **Terminal**.
   2. Enter the following command based on your macOS version:
   
      - **macOS Ventura**:
      ```bash
      sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
      ```
      - **macOS Monterey**:
      ```bash
      sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
      ```
      - **macOS Big Sur**:
      ```bash
      sudo /Applications/Install\ macOS\ Big\ Sur.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume
      ```

   3. Replace `/Volumes/MyVolume` with the name of your USB drive.
   4. Enter your administrator password and wait for the process to complete.

#### For Windows:
If you're using a Windows PC to create the bootable macOS installer, you will need to use additional software like **TransMac** or **balenaEtcher**. Here's how:

1. **Format the USB Drive**:
   1. Download and install **TransMac** from [TransMac Official Site](https://www.acutesystems.com/scrtm.htm) or **balenaEtcher** from [balenaEtcher Official Site](https://www.balena.io/etcher/).
   2. Insert the USB drive.
   3. In **TransMac**, right-click your USB drive and select **Format Disk for Mac**. If you're using **balenaEtcher**, you can skip this step because the software will automatically handle formatting during the process.

2. **Create Bootable Installer**:
   - **Using TransMac**:
     1. Right-click the USB drive in TransMac.
     2. Select **Restore with Disk Image**.
     3. Browse and select your macOS DMG file.
     4. Wait for the process to complete.
   
   - **Using balenaEtcher**:
     1. Open balenaEtcher.
     2. Click **Flash from file** and select the macOS DMG or ISO file.
     3. Select your USB drive as the target.
     4. Click **Flash!** and wait for the process to finish.

### Step 3: Boot from the USB Drive and Reinstall macOS
1. Shut down your Mac and insert the USB drive.
2. Hold the **`Option` key** (Alt) and power on the Mac. Continue holding until you see the **Startup Manager**.
3. Select your USB drive (usually named "Install macOS") to boot from.
4. Once in the **macOS Utilities** window, select **Disk Utility** to erase your disk (if you're doing a clean install), and then choose **Reinstall macOS**.
5. Follow the on-screen instructions to complete the installation.

### Important Notes:
- **Backup Your Data**: If you're erasing your disk, ensure you've backed up important files, such as with Time Machine.
- **Internet Connection**: The macOS installer will verify the software during the installation, so an active internet connection is required.

With these steps, you can successfully reinstall macOS using a bootable USB drive. If you encounter any issues, feel free to troubleshoot or ask for help.
