# network-analysis-commands-tools
A collection of commands and quick notes for working with Windows CMD, Raspberry Pi networking, and related tools.

> Network Analysis & NZYME Setup, Configuration, and Feedback

---
##
## Before  continuing
Carry on for network related commands
or...
Jump to the [Nzyme Specific section](https://github.com/Zkitszo/network-analysis-nzyme/blob/main/README.md#nzyme-specific)
##

## Raspberry Pi: Getting IP Address

Get the IP address of your Pi:
```sh
hostname -I
```

or
```sh
ip addr show
```

Phased out (older command):
```sh
ifconfig
```

### Force 2.4GHz Details
Use the `-4` flag for IPv4 details (specific context may be required).

---

## Network Discovery

**If hostname is known, from Windows CMD (or other PC on the same network):**
```sh
ping <hostname>.local
```

**If using default Raspberry Pi settings:**
```sh
ping raspberrypi.local
```

**Using nmap for subnet scan:**

First, get your computer's IP address:
```cmd
ipconfig
```

Then scan your subnet:
```sh
nmap -sn <ip address/subnet>
# Example:
nmap -sn 192.168.1.0/24
```

**Other useful IP scanning tools:**  
- Angry IP Scanner  
- Fing (mobile, on same network)  
- Advanced IP Scanner  

Watch for device names like `"raspberrypi"` if the default hostname is used.

---

## Windows Command Line: User/Path Prompt

**Obscure CMD username/path:**

Temporary change:
```cmd
prompt $G
```

Permanent change:
```cmd
setx prompt $G
```

Other useful prompt codes:
- `$P`: current drive and full path (default)
- `$N`: current drive only
- `$_`: new line

---
## Nzyme Specific
> Detais and findings as a user setting up Nzyme for the first time directed at those starting with limitted network experiene. You have no choice but to learn.



## Nzyme Node & Tap Files

- Node file:  
  [(https://github.com/nzymedefense/nzyme/releases/download/2.0.0-alpha.17/nzyme-node_rpios-12bookworm-noarch-2.0.0-alpha.17.deb](https://github.com/nzymedefense/nzyme/releases/download/2.0.0-alpha.17/nzyme-node_rpios-12bookworm-noarch-2.0.0-alpha.17.deb)
- Tap file:  
>as of Nov 29, 2025

[(https://github.com/nzymedefense/nzyme/releases/download/2.0.0-alpha.17/nzyme-tap_rpios-12bookworm-arm64-2.0.0-alpha.17.deb](https://github.com/nzymedefense/nzyme/releases/download/2.0.0-alpha.17/nzyme-tap_rpios-12bookworm-arm64-2.0.0-alpha.17.deb)

> Node file is the same for all OS – compare file size.  
> Tap file is also the same across OS setups.

---

## WiFi Adapter Management (Linux/Pi)

**Set default WiFi adapter:**

Confirm current default adapter:
```sh
nmcli
```

Typical interface names:  
- wlan0 (default)
- wlan1, wlan2, wlan3 (additional adapters)

Show configured connections:
```sh
nmcli con show
```

Preconfigured/assigned to wlan0 (credentials stored?)

**Change WiFi adapter:**
```sh
nmcli connection modify <preconfigured-connection> interface-name wlan1
sudo reboot
```

After reboot, defaults to selected adapter.  
If issues arise, repeat `nmcli` checks and modify again as needed.

---

## Contributing

Contribute. New commands, tools/ resoursces or corrections via pull request or issue.
