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

# Mirrors Explained

## Why Mirrors?
Arch Linux packages are distributed via global mirror servers.

* When you install or update, pacman fetches packages from the **top of the mirrorlist**.
* A bad mirror = failed or slow installation.
* A good, nearby mirror = fast, reliable installation.
  That’s why Reflector auto-generates a fast list, but you can still optimize it manually.

<br>

## What Pacstrap Does
`pacstrap` = "Package Bootstrap". It installs Arch into the mounted directory (`/mnt`) using pacman.

* Without this, your disk just has partitions and file systems, but no OS.
* `pacstrap` copies the base system and kernel into your new Arch installation.

<br>

## Why These Packages?
* **base** --> The minimal Arch environment (package manager, shell, init system, libraries).
* **linux** --> The heart of the OS: kernel that interfaces with your hardware.
* **linux-firmware** --> Essential for wireless cards, graphics, sound chips, and storage controllers. Even in VMs, this ensures compatibility.
* **CPU Microcode (intel-ucode / amd-ucode)** --> Patches at CPU level. Fixes vulnerabilities like *Spectre* and *Meltdown*, and prevents random crashes. Always recommended on bare metal.
* **nano** --> Without an editor, you can’t easily modify configs. Vim is available, but nano is simpler.
* **man-db, man-pages, texinfo** --> Arch’s philosophy is “do it yourself”. Manuals are your best help offline, especially in recovery situations.

<br>

## Why Install Now and Not Later?
Because the **live system is temporary**. After reboot, only what’s installed into `/mnt` (via pacstrap) will remain. If you forget microcode or firmware now, you may end up with a non-booting system on bare metal.
