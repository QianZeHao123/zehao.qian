---
title: 'RPi CM4 Notebook'
summary: I have a Raspberry Pi Compute Module 4 and an extend board. This blog will record how I setup it as a docker server.
tags:
  - CS
date: 2025-09-14
---

Raspberry Pi — especially the Raspberry Pi 4 — must be one of the best platforms to learn Linux and Embedded Systems.  
I used to have a Raspberry Pi 4, but I sold it years ago when Bitcoin mining was at its peak — a mining farm bought it from me to use as a controller for their rigs.

# Getting Started with RPi USB Boot on macOS

After that, I decided to dive back into embedded development and got myself a **Compute Module 4 (CM4)** along with a development board that supports **SSD expansion**.  
This setup is perfect for experimenting with Linux, custom boot flows, and high-speed storage.

![https://www.raspberrypi.com/documentation/computers/images/cm4.jpg?hash=dceafc8f655ede304de01430e1ef609d](https://www.raspberrypi.com/documentation/computers/images/cm4.jpg?hash=dceafc8f655ede304de01430e1ef609d)

In this guide, I’ll walk you through how to set up **USB Boot** on macOS to recognize the CM4’s eMMC as a storage device, so you can easily flash system images just like you would with an SD card.

## 1. Install USB Boot Tool

First, clone the official `usbboot` repository:

```bash
git clone --recurse-submodules --shallow-submodules --depth=1 https://github.com/raspberrypi/usbboot
# change disk to usbboot
cd usbboot
```

Here we use --recurse-submodules and --shallow-submodules to download only what is necessary (without full history) to save time and space.

## 2. Install Dependencies

usbboot depends on libusb and pkg-config. You can install them via Homebrew:

```bash
brew install libusb
brew install pkg-config
```

- libusb: A cross-platform library for USB device communication.
- pkg-config: Helps the compiler find the headers and libraries.

## 3. Build libusb from Source (Optional)

If you prefer compiling the latest version from source, you can do so manually:

```bash
curl -OL https://github.com/libusb/libusb/releases/download/v1.0.27/libusb-1.0.27.tar.bz2
tar -xf libusb-1.0.27.tar.bz2
cd libusb-1.0.27
./configure
make
make check
sudo make INSTALL_PREFIX=/usr/local install
```

> Tip: For most users, the Homebrew version is good enough. Manual installation is mainly for debugging or specific version requirements.

## 4. Compile usbboot

Return to the usbboot directory and run:

```bash
make INSTALL_PREFIX=/usr/local
```

This will generate the executable rpiboot.

## 5. Make macOS Recognize the eMMC as a Storage Device

Before running `rpiboot`, make sure your CM4 is in **eMMC boot mode**:

1. On the official CM4 IO Board, locate header **J2** (near the USB port).
2. Use a jumper to short **pin 1 (nRPI_BOOT)** and **pin 2 (GND)**. ![https://www.raspberrypi.com/documentation/computers/images/cm4io.jpg?hash=a69dcc715bfcacfd6dccc10c8d4922c2](https://www.raspberrypi.com/documentation/computers/images/cm4io.jpg?hash=a69dcc715bfcacfd6dccc10c8d4922c2)
3. Connect the USB OTG cable to your computer.
4. Power on the board.

Only after doing this, run:

```bash
sudo ./rpiboot -d mass-storage-gadget64
```

Once it completes, macOS will recognize a new storage device, just like inserting an SD card.
At this point, you can flash an OS image using Raspberry Pi Imager or dd.

---

# Install Docker on Debian-based Systems

Once the OS image is flashed and you can boot into Debian or Raspberry Pi OS, installing Docker allows you to run containers easily.

## Step 1: Remove Conflicting Packages

Remove any older or conflicting versions of Docker and container runtimes:

```bash
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

## Step 2: Add Docker’s Official GPG Key and Repository

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
```

## Step 3: Install Docker

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

```bash
sudo docker run hello-world
```

Now you have successfully installed Docker on your Raspberry Pi. Time to make your hand dirty.