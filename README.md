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
> [!NOTE]
> Running the above gives the actual IP, where some may mistake the command "hostname -i" to return the same, it does not- depending on settup may return a differrent value- for the most part our applications return the callback IP address- using it will just loop the data back to the Pi.

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


## Setting Up PostgrSQL
I faced a fare bit of chgallenges with this- Hopefully this lets you glide past with out any yourself-

> [!IMPORTANT]
> This may be a problematiuc section for less experienced individuals- The documentation lumps the settup into one code bloc- They infact are required to be split up into their oun, individual step, one command at a time, confirming the command's outcome along the way.

> [!CAUTION]
> Care and attention are required with this step- very important it is done, and correctly. Since everything is lumpewd into one code block, you must only run the command. Eerything else is just how it'll loo  once completted. Ex: postgres=# CREATE USER nzyme WITH ENCRYPTED PASSWORD 'YOUR_PASSWORD_HERE';
CREATE ROLE the first postgres=# is just refrencing path and will already be onscreen- Similar the second CREATE ROLE at the end- That is the outcome of the command provided once the command has run-  The correct command here is CREATE USER nzyme WITH ENCRYPTED PASSWORD 'youhavechangedthistoyourpassword';- Following that the command loos like postgres=# GRANT ALL PRIVILEGES ON DATABASE nzyme TO nzyme; GRANT but, the only part you are actually running are GRANT ALL PRIVILEGES ON DATABASE nzyme TO nzyme;- Copying thast whole bloc will not work/ doing it any other way will fail.

> [!WARNING]
> Running the command sudo -u postgres psql returns an error could not change directory to "/home/node000": Permission denied psql (15.14 (Debian 15.14-0+deb12u1)) Type "help" for help. Though, anyone observant wil notice path change to postgres=# which means it wored regardless and you can carry on to the net command. 

> [!IMPORTANT]
> After this part postgres=# \c nzyme You are now connected to database "nzyme" as user "postgres". nzyme=# GRANT CREATE ON SCHEMA public TO nzyme the actual path will show as nzyme=# where in the documentation it remains as postgres=#- The change to nzyme=# is correct.

The code block for the setup of postgsql should be broken out into seperate commands-

> sudo -u postgres psql
> CREATE DATABASE nzyme;
> CREATE USER nzyme WITH ENCRYPTED PASSWORD 'password';
> GRANT ALL PRIVILEGES ON DATABASE nzyme TO nzyme;

>[Note]
> Observe path change- differrent from documentation- yet correct- from postgres=# to nzyme=#- no worries, carry on.

> \c nzyme

...




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

>[Important]
> Sometimes even branded adapters may not be what is advertised. Especially purchasing off 3rd party sites, or market places. Counterfeits, false addvertising, refurbished product where other componnents may have substituted originasls- possible componnent/ chip shortages at time of manufacture for a brief period of time. You may think you are getting an adapter with chipset X when in reality you got your self an adapter with chipset Z. Running commands like 'lsusb' and instead of going by what the advertiser wants you to think is inside, you can retrieve the ID of the device and cross refrence that against a database such as 'WikiDevi'. Using the ID on WikiDevi will help with identifying chipset manufacturer and model. On Windows open the device manager. That ID will be located in Details pane of the device itself, as property 'Hardware IDs'. See https://www.aircrack-ng.org/doku.php?id=compatibility_drivers for further details.




For USB adapters
```sh
lsusb
```

 Fof internal adapters
 ```sh
lspci
```


View capabillities of WiFi adapter(s)

```sh
iw list | grep monitor
```
>[Note]
> The above command 'iw list | grep monitor' returns any instance of the word monitor whether related to monitor mode or not. Running 'iw list' provides an extensive list of ALL your Wifi adapters capabillities, watch for the section which reads 'Supported interface modes'. Alternativelly you can use 'aircrack-ng'.

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

## Use aircrack-ng

Focussing on security, 'Aircrack-ng' is a complete suite of tools to assess WiFi network status.

[(https://www.aircrack-ng.org]
((https://www.aircrack-ng.org/)

## Other

Wep hex dictionary already prepared
[([https://www.aircrack-ng.org/doku.php?id=aircrack-ng
](https://www.latinsud.com/pub/wepdict/)

https://wepattack.sourceforge.net/
https://sourceforge.net/projects/wepattack/

[(Ultra-fast dhcp discovery tool for Linux. Used for LAN reconaissance written in C.](https://www.latinsud.com/pub/dhd/)
'
https://sourceforge.net/projects/prometheus.mirror/
Export metrics for Prometheus
https://github.com/phaag/nfexporter

https://sourceforge.net/projects/zeek.mirror/

https://nfdump.sourceforge.net/   -legacy
https://github.com/phaag/nfdump
http://www.ripe.net/ripe/meetings/ripe-50/presentations/ripe50-plenary-tue-nfsen-nfdump.pdf
nfdump syntax "cheatsheet" for filterring
https://gist.github.com/phaag/06369bed7f39f97e1de51b1b0f5bc29a

NfSen is a graphical WEB based front end for the nfdump netflow tools.
https://sourceforge.net/projects/nfsen/  -legacy
https://github.com/phaag/nfsen





https://sourceforge.net/projects/dragonos-focal/

for Pi
https://sourceforge.net/projects/dragonos-pi64/

https://sourceforge.net/projects/airgeddon.mirror/

iperf

https://sourceforge.net/projects/netstalker.mirror/

Arp spoofer
https://sourceforge.net/projects/elmocut/









---

## Contributing

Contribute. New commands, tools/ resoursces or corrections via pull request or issue.
