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

## Phase II — Storage and Permissions

Storage and Unix permission work was completed and validated on 2026-10-06.

### Shared-Storage Structure

The initial server-data structure under `/srv/storage` is:

```text
/srv/storage/shared
/srv/storage/david
```

Final ownership and permissions:

```text
/srv/storage/shared  root:fileshare   2770
/srv/storage/david   david:fileshare  2770
```

Both directories use the setgid bit (`drwxrws---`) so newly created child
objects inherit the `fileshare` group.

### Group-Inheritance Validation

The `fileshare` group was created for shared-storage access.

A group-context test using `newgrp fileshare` reported GID `1003`.

Test objects created under the shared-storage path inherited the intended
ownership:

```text
testfile  david:fileshare
testdir   david:fileshare
```

This verified group inheritance from the setgid directory.

### Default ACL

A default ACL was applied to `/srv/storage/shared` so group write access remains
consistent for newly created content.

The effective default ACL included:

```text
user::rwx
group::rwx
group:fileshare:rwx
mask::rwx
other::---
```

Fresh validation objects showed:

```text
-rw-rw----  david:fileshare  testfile
drwxrws---+ david:fileshare  testdir
```

The `+` indicates the presence of an extended ACL.

### Phase II Assessment

The storage and filesystem-permissions layer is complete and validated.

With storage and filesystem permissions validated, the next deployment stage
was network exposure and host-firewall policy review before Samba installation.

## Phase III — Host Firewall and Exposure Policy

Carrier's host-firewall policy was designed, tested, and made persistent before
Samba installation.

### Firewall Design

A dedicated native nftables table, `inet tf27`, was created with a default-drop
input policy.

The policy permits only the traffic required by Carrier's current role:

- Established and related traffic
- Loopback traffic
- Traffic arriving through `tailscale0`
- ICMP and ICMPv6 control traffic
- Tailscale transport on UDP `41641`
- DHCP/DHCPv6 client traffic on the Wi-Fi interface
- SSH from the trusted `10.0.0.0/24` LAN
- mDNS/Avahi from the trusted LAN
- KDE Connect TCP/UDP `1714-1764` from the trusted LAN

All other inbound traffic is dropped by default.

Samba ports were intentionally not opened during this phase because Samba had
not yet been installed or configured.

### Tailscale Coexistence

Tailscale already manages its own Netfilter chains through the `iptables-nft`
compatibility layer.

The stock Debian `/etc/nftables.conf` and `nftables.service` were not enabled
because they contain whole-ruleset flush behavior that could remove dynamically
managed Tailscale rules.

Instead, Task Force 27 uses a dedicated rules file:

```text
/etc/nftables.d/tf27.nft
```

and a dedicated systemd unit:

```text
/etc/systemd/system/tf27-firewall.service
```

The service owns only `table inet tf27`.

Startup removes any existing TF27 table and reloads the canonical rules file.
Shutdown removes only the TF27 table. Tailscale-managed tables and chains are
left untouched.

### Validation

The ruleset passed nftables syntax validation before deployment.

Temporary deployment confirmed:

- LAN gateway connectivity
- DNS and Internet resolution
- IPv6 control traffic
- mDNS traffic
- Tailscale coexistence
- SSH access from Sloop to Carrier over the trusted LAN

The LAN SSH rule recorded an accepted connection:

```text
iifname "wlx7419f8174ed0" ip saddr 10.0.0.0/24 tcp dport 22
counter packets 1 bytes 60 accept
```

The firewall was then made persistent with `tf27-firewall.service`.

After a planned reboot, validation confirmed:

```text
systemctl is-enabled tf27-firewall.service
enabled

systemctl is-active tf27-firewall.service
active
```

The `inet tf27` table was restored automatically, Carrier returned to
`10.0.0.40/24`, DNS and Internet access worked, and the Tailscale `ts-input`
hooks remained present for both IPv4 and IPv6.

A second Sloop-to-Carrier SSH test after reboot again incremented the trusted-LAN
SSH rule, confirming that permitted administrative access survived the reboot.

### Phase III Assessment

Carrier now has a persistent default-deny host firewall with explicit service
exposure and verified coexistence with Tailscale.

Firewall deployment and reboot persistence are complete.

## Remaining Deployment Work

The remaining initial file-server deployment work will address:

- Samba installation and share design
- Add explicit Samba exposure rules for the trusted LAN
- Confirm Samba access over Tailscale through the existing trusted
  `tailscale0` policy
- Client access testing from another TF27 system

Backup configuration and restore testing will be handled as a later deployment
phase rather than being combined with initial file-service configuration.

## Status

In progress — storage, permissions, and persistent host firewall complete; Samba deployment next.
