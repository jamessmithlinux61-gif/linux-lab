# LAB-006 — Carrier Server Deployment

## Objective

Prepare Carrier for service as the primary infrastructure host and central
file-storage server for the Task Force 27 environment.

Deployment will proceed in phases so that storage, permissions, network
exposure, remote administration, firewall policy, file services, backup, and
recovery can be designed and validated deliberately.

## Prerequisite Incident

Server deployment was deferred while the following incident remained open:

`INC-002 — Investigate Power Instability on Carrier`

INC-002 was resolved on 2026-09-27 after the probable power fault was identified,
corrected, and validated through hardware testing, extended operation, and
system-journal review.

Carrier was returned to Operational status before server deployment work began.

## Platform

- Hostname: `carrier`
- Platform: Self-built AMD desktop
- Motherboard: MSI A78M-E45 (MS-7721), version 5.0
- Memory: 16 GB
- Operating system: Debian GNU/Linux 13 (trixie)
- Kernel: `6.12.101+deb13-amd64`
- Desktop environment: KDE Plasma
- Architecture: x86-64

Carrier previously used LXQt. The move to KDE Plasma occurred before the server
deployment and is recorded here as part of the system's current operational
baseline.

Server services will be designed so that they do not depend on an interactive
graphical login or KDE session.

## Phase I — Pre-deployment Baseline

Baseline inspection was performed on 2026-09-27 before installation of new
server services.

### Storage

Carrier contains two primary storage devices:

- 500 GB PNY CS900 SSD — operating-system disk
- 1 TB Western Digital HDD — intended server-data disk

The WD disk contains an ext4 filesystem labelled `carrier-storage` and is mounted
at:

`/srv/storage`

Persistent mounting is configured in `/etc/fstab` by UUID:

`d206a318-4fe6-405d-86d5-cafde022b000`

The storage filesystem was approximately 1% used at baseline.

At the beginning of deployment, `/srv/storage` was owned by `david:david` with
mode `755`.

This ownership model is considered temporary. A dedicated group and permissions
model will be designed before shared storage is placed into service.

### Network

Active interfaces at baseline:

- Ethernet: `enp1s0` — down
- Wireless: `wlx7419f8174ed0` — active
- Tailscale: `tailscale0` — active

Carrier was using wireless networking with local IPv4 address:

`10.0.0.40/24`

Carrier's Tailscale IPv4 address was:

`100.121.28.109/32`

Wired Ethernet is preferred for the eventual file-server role where practical,
but conversion to Ethernet is not required before design work continues.

### Remote Administration

OpenSSH Server:

- Enabled at boot
- Active

Tailscale:

- Enabled at boot
- Active

SSH was listening on TCP port 22 on all IPv4 and IPv6 interfaces at baseline.

The existing SSH and Tailscale configuration provides a working remote-
administration foundation, but network exposure will be reviewed as part of the
server firewall design.

### Service Exposure

A listening-service baseline was captured with:

`sudo ss -tulpn`

Before file-server deployment, observed listeners included:

- OpenSSH
- Tailscale
- KDE Connect
- Avahi
- CUPS on loopback only

No Samba file-sharing service had been installed or enabled at this stage.

This baseline will be used for comparison after server services are deployed.

### Firewall

The nftables ruleset was inspected before deployment.

Tailscale-managed chains were present, but the main IPv4 and IPv6 INPUT chains
used an `accept` policy.

Carrier therefore did not have a restrictive host-firewall policy governing
general inbound traffic at baseline.

A deliberate host-firewall policy will be designed before new file services are
placed into normal use.

## Phase I Findings

The pre-deployment baseline established that:

- Carrier is operational following resolution of INC-002.
- The dedicated 1 TB storage disk is mounted persistently at `/srv/storage`.
- The storage filesystem is essentially empty and suitable for planned service.
- The existing ownership model is not yet suitable for shared multi-user storage.
- SSH and Tailscale are enabled, active, and persistent.
- Carrier is currently networked primarily through Wi-Fi.
- No restrictive general host-firewall policy is currently in effect.
- No Samba service is currently present.
- Current listening services have been documented for later comparison.

## Planned Phase II

The next deployment phase will address:

- Server storage directory structure
- Linux users and groups for shared access
- Unix ownership and permission model
- Samba installation and share design
- LAN versus Tailscale service exposure
- Host-firewall policy
- Client access testing from another TF27 system

Backup configuration and restore testing will be handled as a later deployment
phase rather than being combined with initial file-service configuration.

## Status

In progress — Phase I baseline complete.
