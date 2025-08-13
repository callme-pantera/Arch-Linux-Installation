<div align="center">
  <div style="text-align: center;">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="/assets/images/logos/pantera-1.4.png">
      <source media="(prefers-color-scheme: light)" srcset="/assets/images/logos/pantera-1.3.png">
      <img src="/assets/images/logos/pantera-1.4.png" alt="Logo of Pantera" width="350px">
    </picture>
  </div>

  <br>

  [![Issues Badge](https://img.shields.io/badge/ISSUES-0-Test?style=for-the-badge&logo=https%3A%2F%2Ficons8.com%2Ficon%2F83178%2Fimage-file&labelColor=%23333333&color=%23ba181b)](https://github.com/callme-pantera/Arch-Linux-Installation/issues)
  [![Stars Badge](https://img.shields.io/badge/STARS-1-Test?style=for-the-badge&logo=https%3A%2F%2Ficons8.com%2Ficon%2F83178%2Fimage-file&labelColor=%23333333&color=%23f6aa1c)](https://github.com/callme-pantera/Arch-Linux-Installation/stargazers)
  [![Commits Badge](https://img.shields.io/github/commit-activity/m/callme-pantera/Arch-Linux-Installation?style=for-the-badge&label=COMMITS&logo=https%3A%2F%2Ficons8.com%2Ficon%2F83178%2Fimage-file&labelColor=%23333333&color=%237678ED)](https://github.com/callme-pantera/Arch-Linux-Installation/commits/main/)
  [![License Badge](https://img.shields.io/badge/LICENSE-CC-Test?style=for-the-badge&logo=https%3A%2F%2Ficons8.com%2Ficon%2F83178%2Fimage-file&labelColor=%23333333&color=%234361ee)](../LICENSE)
</div>

<br>

# Installation and Configuration
In this documentation, Arch Linux is **installed in a virtual machine** for testing purposes. I downloaded the ISO file from the [official website](https://archlinux.org/download/).<br>

If you are new to Arch, I strongly recommend starting in a VM rather than installing it directly on your computer (*bare-metal installation*). In a VM, you can take snapshots and revert to a working state if something goes wrong. On a physical system, a single mistake during installation can break the OS and make recovery more complicated than necessary.<br>

For completeness, I’ve also included general notes on how the process would differ for a bare-metal installation. You can return to this documentation after completing the initial setup in a VM or after following a dedicated guide for local installation, which can be found in the Comprehensive Breakdown folder as "bare-metal-prep" or directly [here](../comprehensive%20breakdown/bare-metal-prep.md).

<br>

> [!WARNING]  
> If you are not familiar with Arch Linux, avoid installing it directly on bare metal. Start in a VM, get comfortable with the system, and only move to a physical installation once you’re confident in the process. If you choose to install bare-metal without prior experience, you do so entirely at your own risk — you’ve been warned.

<br>

In my case, this VM is hosted in my [**CSL**](https://github.com/callme-pantera/CSL-Prototype) (*Cybersecurity Lab*) using **Proxmox**. You can use any virtualization platform you prefer, such as **VMware Workstation**, **VirtualBox**, **Hyper-V**, or similar — all will work for this setup.

<br>

## VM Configuration
Since my [**CSL**](https://github.com/callme-pantera/CSL-Prototype) has **ample resources**, I allocated more than the average recommended specs.
This ensures the VM runs as close to bare-metal performance as possible — even with heavy ricing, compiling from the AUR, or multitasking with multiple desktop environments.


| Resource         | Minimum Specs           | High-Performance Setup (Used) | Notes                                                                                                 |
| ---------------- | ----------------------- | ----------------------------- | ----------------------------------------------------------------------------------------------------- |
| **CPU Sockets**  | 1                       | 1                             | 1 socket is optimal for VM scheduling efficiency.                                                     |
| **CPU Cores**    | 4                       | 8                             | More cores drastically speed up package compilation and multitasking.                                 |
| **RAM**          | 8 GB                    | 16 GB                         | High RAM ensures smooth performance with multiple DE/WM setups and heavy applications.                |
| **Storage**      | 40 GB (SSD)             | 100 GB (SSD-backed)           | Extra space for themes, AUR builds, and snapshots; SSD significantly improves install and load times. |
| **Swap**         | 2 GB                    | 2 GB                          | Small safety net; rarely used with high RAM allocation.                                               |
| **GPU / Video**  | VirtIO / VMware SVGA II | VirtIO / VMware SVGA II       | Use with 3D acceleration enabled for smooth animations and compositor effects.                        |
| **Network**      | VirtIO (Bridged)        | VirtIO (Bridged)              | Provides low-latency networking and fast package downloads.                                           |
| **Display VRAM** | 64 MB                   | 256 MB                        | Higher VRAM improves rendering of animations and high-resolution wallpapers.                          |
| **NUMA**         | Disabled                | Enabled                       | Improves CPU and memory allocation efficiency on multi-core, multi-RAM-node systems.                  |
| **CPU Type**     | Default                 | x86-64-v4                     | Uses the most modern instruction set supported by the hardware for maximum performance.               |

<br>

**Why this matters:**
With **8 cores + 16 GB RAM**, backed by **NUMA** and **x86-64-v4**, this VM can:

- Compile large AUR packages quickly (e.g., Firefox, KDE Plasma)
- Run compositors like `picom` or full desktop environments with animations and no lag
- Host multiple DEs/WMs for testing without impacting responsiveness
- Fully leverage the server’s **SSD-backed 2 TB storage** for storing large themes, icon sets, and VM snapshots

<br>

<div>
    <img src="/assets/images/VM-specs.png" style="width: 100%";>
</div>

<br>

## Pre-Installation
The pre-installation stage can be thought of as preparing the foundation of the operating system before any actual software is installed. This involves setting up the essential environment — such as partitioning the disk, formatting file systems, and mounting the target directories — so that the installation process has a proper location to work with. Without this base, there would be no place to execute the installation or store the system files.
