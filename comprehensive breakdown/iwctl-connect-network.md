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

# Connecting to a Network with `iwd` - iNet Wireless Daemon
* Lightweight wireless daemon for Linux developed by Intel.
* Uses kernel features directly to reduce dependencies.
* Works standalone or with network managers (ConnMan, NetworkManager, systemd-networkd).
* **Do not follow iwd-specific instructions if using it through a network manager**, unless the manager's docs say so.

---

### 1.1 iwctl basics
* Start prompt: `iwctl` --> `[iwd]#` *prompt*.
* Autocomplete with `Tab`. Exit with `Ctrl+D`.
* Commands can also be run directly:

  ```bash
  $ iwctl device wlan0 show
  ```

<br>

### 2.1.1 Connect to Wi-Fi
1. List devices:

   ```
   device list
   ```
2. Power on adapter if needed:

   ```
   device <name> set-property Powered on
   adapter <adapter> set-property Powered on
   ```
3. Scan:

   ```
   station <name> scan
   ```
4. List networks:

   ```
   station <name> get-networks
   ```
5. Connect:

   ```
   station <name> connect <SSID>
   ```

   * Hidden network: `connect-hidden`
   * Passphrase: prompt or `--passphrase <pass>`
   * Credentials stored in `/var/lib/iwd`.

<br>

### 2.1.2 WPS/WSC Connection
* Check if supported: `wsc list`
* Push-button connect:

  ```
  wsc <device> push-button
  ```
* Optional: PIN mode via `wsc` command.

<br>

### 2.1.3 Disconnect
```
station <device> disconnect
```

<br>

### 2.1.4 Show Info
* Device details (MAC, etc.):

  ```
  device <device> show
  ```
* Connection state:

  ```
  station <device> show
  ```

<br>

### 2.1.5 Manage Known Networks
* List: `known-networks list`
* Forget: `known-networks <SSID> forget`

<br>

## 3. Network Configuration
* Stored in `/var/lib/iwd/<SSID>.<type>`
  * `.psk`, `.open`, `.8021x`
* WPA-PSK: store `Passphrase` or `PreSharedKey`.
* WPA Enterprise: supports **EAP-PWD**, **EAP-PEAP**, **TTLS-PAP**, **EAP-TLS**.
* **eduroam**: `.8021x` file with EAP method and credentials.
* Supports embedded PEM certs inside config.

<br>

## 4. Optional Configuration (`/etc/iwd/main.conf`)
* Disable autoconnect per network: `[Settings] AutoConnect=false`
* Disable periodic scan:

  ```
  [Scan]
  DisablePeriodicScan=true
  ```
* Built-in DHCP:

  ```
  [General]
  EnableNetworkConfiguration=true
  ```
* IPv6: `[Network] EnableIPv6=true` (or false)
* Static IP example:

  ```
  [IPv4]
  Address=192.168.1.10
  Netmask=255.255.255.0
  Gateway=192.168.1.1
  DNS=192.168.1.1
  ```
* DNS manager: `NameResolvingService=systemd` or `resolvconf`.




