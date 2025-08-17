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


| Resource         | Minimum Specs           | High-Performance Setup (Used)  | Notes                                                                                                 |
|:-----------------|:------------------------|:-------------------------------|:------------------------------------------------------------------------------------------------------|
| **CPU Sockets**  | 1                       | 1                              | 1 socket is optimal for VM scheduling efficiency.                                                     |
| **CPU Cores**    | 4                       | 8                              | More cores drastically speed up package compilation and multitasking.                                 |
| **RAM**          | 8 GB                    | 16 GB                          | High RAM ensures smooth performance with multiple DE/WM setups and heavy applications.                |
| **Storage**      | 40 GB (SSD)             | 100 GB (SSD-backed)            | Extra space for themes, AUR builds, and snapshots; SSD significantly improves install and load times. |
| **Swap**         | 2 GB                    | 2 GB                           | Small safety net; rarely used with high RAM allocation.                                               |
| **GPU / Video**  | VirtIO / VMware SVGA II | VirtIO / VMware SVGA II        | Use with 3D acceleration enabled for smooth animations and compositor effects.                        |
| **Network**      | VirtIO (Bridged)        | VirtIO (Bridged)               | Provides low-latency networking and fast package downloads.                                           |
| **Display VRAM** | 64 MB                   | 256 MB                         | Higher VRAM improves rendering of animations and high-resolution wallpapers.                          |
| **NUMA**         | Disabled                | Enabled                        | Improves CPU and memory allocation efficiency on multi-core, multi-RAM-node systems.                  |
| **CPU Type**     | Default                 | host / x86-64-v4               | `host` matches the host CPU exactly for max performance; `x86-64-v4` enables the latest instruction set supported. |


<br>

**Why this matters:**
With **8 cores + 16 GB RAM**, backed by **NUMA** and an optimized CPU type (`host` or `x86-64-v4`), this VM can:

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
The pre-installation stage can be thought of as preparing the foundation of the operating system before any actual software is installed. This involves setting up the essential environment — such as partitioning the disk, formatting file systems, and mounting the target directories — so that the installation process has a proper location to work with. Without this base, there would be no place to execute the installation or store the system files.<br>

Boot Arch Linux according to your setup: if you’re using a VM, start the virtual machine; if you’re installing on bare metal, insert the USB stick, set it as the first boot device in the BIOS, and begin the pre-installation process.<br>

<div>
  <img src="/assets/images/ArchLinux-Boot.png" style="width: 100%";>
  <img src="/assets/images/ArchLinux-Boot2.png" style="width: 100%";>
</div>

<br>

### Console Keyboard Layout and Font Customization
When the pre-installation process begins and Arch Linux boots, you’ll be dropped into a terminal. At this stage, you can configure your keyboard layout to match your preferred language and layout.<br>

```bash
# Lists all available keyboard layouts
localectl list-keymaps

# Example: set German keyboard layout
loadkeys de-latin1
```

If you are unsure of the exact layout name, run `localectl list-keymaps` and search within the list (you can use `grep` to filter, e.g., `localectl list-keymaps | grep de`).<br>

After configuring your keyboard layout, you can also change the font used in your terminal. This step is optional, but it demonstrates one of Arch Linux’s strengths — **the ability to customize even the smallest details** of your system. Console fonts are stored in `/usr/share/kbd/consolefonts/`. To explore the available fonts:<br>

```bash
# Browse through all console fonts
ls /usr/share/kbd/consolefonts | less
```

<br>

Additionally you can preview a font without permanently changing your configuration. Simply run:<br>

```bash
# Temporary preview
setfont ter-v16n
setfont lat9w-16
```

- `setfont` applies the font instantly.
- The change lasts **only until you reboot** or switch consoles.
- The `.psf.gz` file extension and full path are optional when using `setfont`.

Once you find a font you like, you can make it the default by editing the `/etc/vconsole.conf` file:<br>

```bash
nano /etc/vconsole.conf

# Add or edit the following line:
FONT=ter-v16n
```

Save the file and reboot to apply the change.

<br>

### Optional: Verify Boot Mode
You can check which boot mode your system is using during installation. The firmware’s bitness (*UEFI 32-bit or 64-bit, or Legacy BIOS*) must be compatible with the boot loader you plan to install.<br>

Run:

```bash
cat /sys/firmware/efi/fw_platform_size
```

- `64` --> 64-bit UEFI mode (recommended)
- `32` --> 32-bit UEFI mode (rare, older hardware)
- *No such file or directory* --> Legacy BIOS mode

<br>

#### Switching a Proxmox VM from Legacy BIOS to UEFI
In my case, running:

```bash
cat /sys/firmware/efi/fw_platform_size
```

returned:

```
No such file or directory
```

This meant the VM was **not booting in UEFI mode**, but instead using **Legacy BIOS** (SeaBIOS). This is not necessarily a problem — Legacy BIOS is simply older technology. Compared to UEFI, it lacks certain modern features and can be slightly slower, but it works fine for most cases.<br>

However, for the sake of this project and for better alignment with modern hardware, I decided to **switch the VM to UEFI**.<br>

1. **Shut down the VM** completely.
   * Do not just restart it; make sure it’s powered off.

2. **Add an EFI Disk**
   * In the Proxmox web interface, go to:
     **VM --> Hardware --> Add --> EFI Disk**
   * **Storage**: Select a fast storage location (e.g., `local-lvm` or `local`)
   * **Size**: Leave at default (usually 128 KB)
   * Confirm.

3. **Change BIOS type**
   * Go to **VM --> Options --> BIOS**
   * Change from **SeaBIOS** to **OVMF (UEFI)**.
   * Save the change.

4. **Check the machine type**
   * While still in **Options**, set **Machine Type** to `q35` (recommended for UEFI VMs).
   * This improves hardware compatibility and supports PCIe devices better.

5. **Verify boot order**
   * Go to **VM --> Options --> Boot Order**.
   * Ensure the **CD/DVD drive with your Arch ISO** is first in the list.
   * Move the EFI Disk above the hard drive if possible.

6. **Start the VM**
   * Boot normally.
   * The VM should now start in UEFI mode.

<br>

<div>
  <img src="/assets/images/VM-specs-update-BIOS.png" style="width: 100%";>
</div>

<br>

Once you restart the VM, you may be unable to boot into the Arch Linux installer if **Secure Boot** is enabled. To resolve this:

1. Open the **Device Manager** in the UEFI firmware interface.
2. Navigate to **Secure Boot Configuration**.
3. Disable **Secure Boot**.
4. Save the changes and select **Continue** to proceed with booting.

After the system starts, you can verify that UEFI boot mode is active by running this command again:

```bash
cat /sys/firmware/efi/fw_platform_size
```

If it returns `64` or `32`, you are in UEFI mode. If it returns *No such file or directory*, the system is still booted in Legacy BIOS mode.

<br>

<div>
  <img src="/assets/images/UEFI-secure-boot1.png" style="width: 100%";>
  <img src="/assets/images/UEFI-secure-boot2.png" style="width: 100%";>
  <img src="/assets/images/ArchLinux-Boot3-success.png" style="width: 100%";>
</div>

<br>

### Connect to the Internet
These are the steps to ensure you have a intternet connection:<br>

1. Ensure that your network adapter is listed and enabled
  ```bash
  ip link
  ```
2. Connect to the network
   - Ethernet: Plug in the cable
   - Wi-Fi: Authenticate to the wireless network using [iwd](/comprehensive%20breakdown/iwctl-connect-network.md).
3. Configure your network connection
   - DHCP: dynamic IP address and DNS server assignment should work out of the box for Ethernet, WLAN, and WWAN network interfaces.
   - Static IP address: follow Network configuration [Assign static IP address](/comprehensive%20breakdown/assign-static-ip.md).
4. Verify the connection with a simple ping
   ```bash
   ping archlinux.org
   ```

<br>

<div>
  <img src="/assets/images/internet-success-boot.png" style="width: 100%";>
</div>

<br>

> [!NOTE]
> Since this Arch installation is running on a VM inside my CSL, no additional network setup was required. The Lab environment is already configured to provide internet connectivity to any VM that is assigned to the correct VLAN and connected to the appropriate Linux bridge.

<br>

### Update the System Clock
In the Arch Linux live environment, **`systemd-timesyncd`** is enabled by default. This means the system clock will be automatically synchronized with internet time servers as soon as an internet connection is established.<br>

You can verify the time status using:

```bash
timedatectl
```

Look for `System clock synchronized: yes` to confirm that synchronization is active. If the clock is **not synchronized**, you can enable and start the time synchronization service:

```bash
timedatectl set-ntp true
```

<div>
  <img src="/assets/images/timedatectl.png" style="width: 100%";>
</div>

<br>

#### Manually Changing the Time and Timezone
Arch uses `timedatectl` (*part of systemd*) to handle timezones. To list all available timezones, use this command:

```bash
timedatectl list-timezones
```

<br>

> [!TIP]
> You can scroll or search with `/` if the list is long.

<br>

To set your desired timezone, use this command:

```bash
sudo timedatectl set-timezone <timezone name>

# To verify the change again
timedatectl
```

<div>
  <img src="/assets/images/timedatectl-timezone.png" style="width: 100%";>
</div>

<br>

While automatic synchronization is preferred, you can manually set the system date and time using `timedatectl`:

```bash
# Set date and time (format: YYYY-MM-DD HH:MM:SS)
sudo timedatectl set-time "2025-08-14 19:30:00"
```

To change only the date:

```bash
sudo timedatectl set-time "2025-08-14"
```

To change only the time:

```bash
sudo timedatectl set-time "19:30:00"
```

<br>





