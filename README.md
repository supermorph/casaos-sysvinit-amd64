# CasaOS SysVinit Package

## Overview
This package ports CasaOS v0.4.15 to SysVinit-based systems (Devuan, MX Linux, AntiX, older Debian/Ubuntu).

## Features
- ✅ LSB-compliant init scripts
- ✅ Manual dependency management with auto-start
- ✅ All 6 CasaOS microservices included
- ✅ Complete web UI (39MB)
- ✅ Auto-start on boot
- ✅ Proper service ordering with sequence numbers

## Package Files

1. **casaos-sysvinit_0.4.15.deb** (56MB) - For Debian-based systems
2. **casaos-sysvinit-0.4.15.tar.gz** (59MB) - Universal tarball
3. **install-sysvinit.sh** - Universal installer script (auto-detects OS)

## Package Contents
- 6 SysVinit init scripts in `/etc/init.d/`
- 6 CasaOS binaries in `/usr/bin/`
- Configuration files in `/etc/casaos/`
- Web UI in `/var/lib/casaos/www/`

## Services Included
1. casaos-message-bus - Inter-service communication
2. casaos-gateway - API gateway and web server
3. casaos-user-service - User authentication
4. casaos-local-storage - File management
5. casaos-app-management - Docker/app management
6. casaos - Main service

## Installation

### Quick Install (Recommended)
```bash
# Universal installer - auto-detects your system
sudo bash install-sysvinit.sh
```

### Manual - Debian-based systems
```bash
sudo dpkg -i casaos-sysvinit_0.4.15.deb
```

### Manual - Universal (any SysVinit system)
```bash
# Extract tarball
cd / && sudo tar xzf /path/to/casaos-sysvinit-0.4.15.tar.gz

# Make init scripts executable
sudo chmod +x /etc/init.d/casaos*

# Enable at boot (choose one method)
# Method 1: update-rc.d (Debian/Ubuntu)
for svc in casaos-message-bus casaos-gateway casaos-user-service casaos-local-storage casaos-app-management casaos; do
    sudo update-rc.d $svc defaults
done

# Method 2: insserv (older systems)
for svc in casaos-message-bus casaos-gateway casaos-user-service casaos-local-storage casaos-app-management casaos; do
    sudo insserv $svc
done

# Method 3: chkconfig (RHEL/CentOS)
for svc in casaos-message-bus casaos-gateway casaos-user-service casaos-local-storage casaos-app-management casaos; do
    sudo chkconfig $svc on
done

# Start services in order
for svc in casaos-message-bus casaos-gateway casaos-user-service casaos-local-storage casaos-app-management casaos; do
    sudo /etc/init.d/$svc start
    sleep 2
done
```

## Usage
```bash
# Start/stop services
sudo /etc/init.d/casaos start
sudo /etc/init.d/casaos stop
sudo /etc/init.d/casaos restart
sudo /etc/init.d/casaos status

# Or use service command (if available)
sudo service casaos start
sudo service casaos stop
sudo service casaos restart
sudo service casaos status

# View all services
ls /etc/init.d/casaos*
```

## SysVinit Features
- LSB init info headers for proper dependency tracking
- Manual dependency startup in main casaos script
- Sequential boot ordering with S20-S25 sequence numbers
- Standard start/stop/restart/status commands
- Compatible with update-rc.d, insserv, and chkconfig

## Service Dependencies
The main `casaos` service automatically starts required dependencies:
1. casaos-message-bus (S20)
2. casaos-gateway (S21)
3. casaos-user-service (S22)
4. casaos-local-storage (S23)
5. casaos-app-management (S24)
6. casaos (S25)

## Access
Web interface: http://localhost (or your server IP)

## Compatibility
- Devuan (all versions)
- MX Linux
- AntiX
- Debian (pre-Jessie without systemd)
- Ubuntu (pre-15.04 with sysvinit)
- Any SysVinit-based distribution

## Differences from Other Versions
- **vs systemd**: Uses init.d scripts instead of unit files
- **vs OpenRC**: Uses LSB headers instead of depend() blocks
- **vs Upstart**: Uses shell scripts instead of declarative configs

Version Ported **Date:** 2 December 2025
Build Version: 0.4.15
License: Apache 2.0

## Maintainer Information

**Original Package Maintainer:** CasaOS Team <support@casaos.io>

**Maintainer of these ported packages:** Nobody really - these are just working proof of concepts. Well, I fixed it to work on sysvinit and ported that one to the other init systems.

Initially I just wanted CasaOS to work on Devuan Linux so I could make much more efficient bootable/installable images I can restore/install to my own home computers etc.

**PLEASE NOTE:** From the very first moment you ponder to try these, make a backup of the OS state and take it that **there's no responsibility of any description** because they were just for me and I am sharing for anyone to look at.

All original coding I didn't port belongs to the copyrighted maker/creator.

**P.S.** IceWhale/Zema: I didn't modify any API stuff so it uses your API as it was programmed originally, so please, don't sue - I'm just tryna make the proof of concept available for others to ponder.

**P.S.S.** I have help in parts with Claude AI, for the tricky sections. I'm no coder but I do know my way around a penguin-based OS heh.
