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

# Partitioning, Formating and Mounting the Disk
When installing Arch Linux (or any modern Linux distribution), the first crucial step is preparing the **disk layout**. This means dividing the physical (or virtual) disk into logical partitions, each serving a specific purpose.<br>

In my case, I have a **clean 100 GiB virtual disk** (`/dev/sda`) provided by Proxmox. Since this is a fresh VM with no data, I can safely erase and repartition the disk from scratch. I also want to use **UEFI boot with GPT (GUID Partition Table)**, because this is the modern standard and provides better flexibility than the old BIOS/MBR system.

<br>

## Step 1: Partition the Disk with `fdisk`
Create a new GPT partition table
   * Command: `g`
   * Why? GPT is the modern replacement for MBR. It supports larger disks, more partitions (up to 128 by default), and is required for UEFI booting. Since my VM is set to use **OVMF (UEFI firmware)**, GPT is mandatory.

## Step 2: EFI System Partition (ESP)
* Command: `n` → Partition number `1` → Default first sector → Last sector: `+1G`.
* Change type with `t` → select partition `1` → enter `EFI System`.

Result: `/dev/sda1` with size **1 GiB**.

**Why do we need this?**
The EFI System Partition (ESP) is where the bootloader and UEFI firmware files live. UEFI firmware looks here first to find a bootable OS.

* **Size choice (1 GiB):**
  Technically, ESP only needs around 200–300 MiB. However, allocating **1 GiB** ensures plenty of space for multiple bootloaders, kernels, or future expansion. Since the disk is 100 GiB, the extra space used here is negligible.

## Step 3: Swap Partition
* Command: `n` → Partition number `2` → Default first sector → Last sector: `+4G`.
* Change type with `t` → select partition `2` → enter `Linux swap`.

Result: `/dev/sda2` with size **4 GiB**.

**Why swap?**
Swap space acts as “overflow” memory when RAM runs out. On modern systems with lots of RAM, swap may rarely be used, but it still has benefits:

* Provides stability under memory pressure.
* Required for **hibernation** (suspend-to-disk).
* Helps avoid crashes if processes spike RAM usage.

**Size choice (4 GiB):**
The traditional rule of thumb was *swap = RAM or 2×RAM*. Today this is outdated. With 16 GiB RAM in my VM, I don’t need a huge swap. **4 GiB is enough** for safety, while not wasting space. If hibernation were planned, swap would need to be at least as large as RAM (16 GiB).

## Step 4: Root Partition `/`
* Command: `n` → Partition number `3` → Default first sector → Default last sector (use all remaining space).
* Type defaults to `Linux filesystem`.

Result: `/dev/sda3` with size ≈ **95 GiB**.

**Why root?**
The root (`/`) partition is the **core of the Linux system**. Everything is stored here: binaries, configuration, libraries, user files, etc.

**Size choice (≈95 GiB):**
This takes the rest of the disk after ESP and swap. For a VM lab system, \~95 GiB is plenty for Arch, installed packages, logs, and extra data. If I wanted, I could create separate partitions for `/home` or `/var`, but for simplicity in a test lab, a single root partition is easiest and flexible.

## Step 5: Save the Layout
* Command: `w` → writes changes to disk and exits `fdisk`.

Now the disk has 3 partitions:

* `/dev/sda1` → EFI System Partition (1 GiB)
* `/dev/sda2` → Swap (4 GiB)
* `/dev/sda3` → Root (95 GiB)

## Step 6: Format the Partitions

Now that partitions exist, each needs to be formatted with a filesystem (or initialized, in the case of swap)<br>

**Why these choices?**
* **FAT32 for EFI:** Required by UEFI. Firmware won’t boot from anything else.
* **Swap:** Not a filesystem, just a reserved space for virtual memory.
* **ext4 for root:** Stable, well-supported, mature Linux filesystem. Alternatives exist (btrfs, xfs, zfs), but ext4 is recommended for beginners and servers due to reliability.

## Step 7: Mount the Partitions
Finally, mount the partitions so Arch can be installed:

```bash
# Mount root filesystem
mount /dev/sda3 /mnt

# Mount EFI system partition
mkdir -p /mnt/boot
mount /dev/sda1 /mnt/boot
```

At this point:
* Root is mounted at `/mnt`
* EFI is mounted at `/mnt/boot`
* Swap is enabled and in use

