<div align="center">
  <div style="text-align: center;">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="../assets/images/logos/pantera-1.4.png">
      <source media="(prefers-color-scheme: light)" srcset="../assets/images/logos/pantera-1.3.png">
      <img src="../assets/images/logos/pantera-1.4.png" alt="Logo of Pantera" width="350px">
    </picture>
  </div>

  <br>

  [![Issues Badge](https://img.shields.io/badge/ISSUES-0-Test?style=for-the-badge&logo=https%3A%2F%2Ficons8.com%2Ficon%2F83178%2Fimage-file&labelColor=%23333333&color=%23ba181b)](https://github.com/callme-pantera/CSL-prototype/issues)
  [![Stars Badge](https://img.shields.io/badge/STARS-1-Test?style=for-the-badge&logo=https%3A%2F%2Ficons8.com%2Ficon%2F83178%2Fimage-file&labelColor=%23333333&color=%23f6aa1c)](https://github.com/callme-pantera/CSL-prototype/stargazers)
  [![Commits Badge](https://img.shields.io/github/commit-activity/m/callme-pantera/CSL-prototype?style=for-the-badge&label=COMMITS&logo=https%3A%2F%2Ficons8.com%2Ficon%2F83178%2Fimage-file&labelColor=%23333333&color=%237678ED)](https://github.com/callme-pantera/CSL-prototype/commits/main/)
  [![License Badge](https://img.shields.io/badge/LICENSE-CC-Test?style=for-the-badge&logo=https%3A%2F%2Ficons8.com%2Ficon%2F83178%2Fimage-file&labelColor=%23333333&color=%234361ee)](LICENSE)
</div>

<br>

# Bare-Metal Preparation
Installing Arch Linux directly on your hardware (*bare metal*) requires some preparation to ensure the process goes smoothly.

## 1. Quick Compatibility Checklist
Before you start, make sure your system meets these requirements:

* **64-bit CPU** (x86_64 architecture)
* **UEFI firmware** (recommended; Legacy BIOS works but is less common for modern systems)
* **UEFI Secure Boot disabled** (Arch does not ship Microsoft-signed bootloaders)
* **At least 2 GB RAM** *(8 GB+ recommended for a smooth experience)*
* **Reliable internet connection** (wired or wireless supported by Linux)
* **An empty or backed-up target drive** (installation will erase all data)
* **USB port** and an 8 GB or larger USB stick for the installer

<br>

## 2. Download the Official ISO
Get the latest Arch Linux ISO from the [official download page](https://archlinux.org/download/).

<br>

## 3. Create a Bootable USB Drive
**Windows** – Using **Rufus**:

1. Open Rufus and select your USB stick.
2. Select the downloaded ISO file.
3. Set **Partition Scheme** to **GPT** (for UEFI systems).
4. File system: **FAT32**.
5. Start the process.

**Linux** – Using `dd`:

```bash
sudo dd if=archlinux.iso of=/dev/sdX bs=4M status=progress oflag=sync
```

*(Replace `/dev/sdX` with your USB device path — double-check before running.)*

Other tools: **balenaEtcher**, **Ventoy** (supports multi-boot USBs).

<br>

## 4. Disable Secure Boot
Arch Linux does not include Microsoft-signed bootloaders, so most UEFI firmware will block it if Secure Boot is enabled.

* Enter your BIOS/UEFI settings (usually **DEL**, **F2**, or **ESC** at boot).
* Locate the **Secure Boot** option and set it to **Disabled**.

If you skip this step, your system may refuse to boot the installer with an error like *"Unauthorized bootloader"* or *"Secure Boot violation"*.

<br>

## 5. Boot from the USB Stick

1. Plug the USB stick into the target PC.
2. Open the **boot menu** (often **F12**, **F10**, or **ESC** depending on the motherboard).
3. Select the USB device.
4. Boot into the Arch Linux live environment.

<br>

## 6. Proceed with Installation
From here, the installation steps are the same as in a virtual machine — except you will partition and install directly to your physical drive.

> [!WARNING]
> Installing Arch Linux on bare metal will erase all data on the target drive. Make sure to back up important files before continuing. Once the process starts, it cannot be undone.


