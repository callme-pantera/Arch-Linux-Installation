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

# Static IP Configuration (Manual)
If your network does not use DHCP, you must manually configure:

1. **IP address**
2. **Routing table**
3. **DNS servers**

The `iproute2` tools (already in `base`) are used for this.

---

## 1. Find Your Interface Name
List network interfaces:

```bash
ip link
```

or

```bash
ls /sys/class/net
```

Example names:

* Ethernet: `enp3s0`, `eth0`
* Wireless: `wlp2s0`, `wlan0`

<br>

## 2. Assign the Static IP

```bash
sudo ip address add 192.168.1.100/24 dev enp3s0
```

**Notes:**

* Use `broadcast +` to auto-calculate the broadcast address:

  ```bash
  sudo ip address add 192.168.1.100/24 broadcast + dev enp3s0
  ```
* Avoid conflicting with DHCP ranges.

To remove:

```bash
sudo ip address del 192.168.1.100/24 dev enp3s0
```

<br>

## 3. Set the Default Gateway

```bash
sudo ip route add default via 192.168.1.1 dev enp3s0
```

View routes:

```bash
ip route show
```

Delete a route:

```bash
sudo ip route del default via 192.168.1.1
```

<br>

## 4. Configure DNS
**Temporary (until reboot):**

```bash
echo "nameserver 1.1.1.1" | sudo tee /etc/resolv.conf > /dev/null
```

**Persistent:**
If using `systemd-resolved`:

```bash
sudo systemctl enable --now systemd-resolved
sudo resolvectl dns enp3s0 1.1.1.1
```

<br>

## 5. Make It Persistent Across Reboots
The above `ip` commands reset after reboot. Options for persistence:

* **systemd-networkd** (*recommended for minimal setups*)
* **NetworkManager**
* **netctl**
* `/etc/rc.local` (*less clean, legacy method*)

**Example with systemd-networkd**:
Create `/etc/systemd/network/20-wired.network`:

```ini
[Match]
Name=enp3s0

[Network]
Address=192.168.1.100/24
Gateway=192.168.1.1
DNS=1.1.1.1
```

Then:

```bash
sudo systemctl enable --now systemd-networkd
sudo systemctl enable --now systemd-resolved
```

---

## 6. Verify

```bash
ip address show dev enp3s0
ip route show
ping -c 3 1.1.1.1                   # test connectivity
ping -c 3 archlinux.org             # test DNS
```

<br>

# Quick Command Summary

```bash
# Assign static IP
ip addr add 192.168.1.100/24 dev enp3s0

# Set default gateway
ip route add default via 192.168.1.1 dev enp3s0

# Temporary DNS
echo "nameserver 1.1.1.1" > /etc/resolv.conf

# Show config
ip a
ip r
```



