# LAB-005 — Sister Linux Mint Deployment

## Objective

Deploy Linux Mint Cinnamon on a family member's Lenovo laptop as a stable,
Windows-like daily-use workstation.

## Hardware

- Manufacturer: Lenovo
- Model: IdeaPad 3 15IML05
- Machine type: 81WR
- Internal storage: 238.5 GiB SSSTC CL1-4D256 NVMe SSD
- Hostname: `sevilla`

## Operating System

- Linux Mint 22.3 Cinnamon 64-bit
- Final kernel: `7.0.0-31-generic`
- UEFI installation
- Single-boot Linux system

## Pre-deployment Validation

- Booted Linux Mint live environment successfully in UEFI mode.
- Confirmed Wi-Fi connectivity and DNS resolution.
- Verified Internet connectivity with:

  `ping -c 4 debian.org`

- Initial result: 4 packets transmitted, 4 received, 0% packet loss.
- Identified installer media and target disk with:

  `lsblk -o NAME,SIZE,FSTYPE,TYPE,MOUNTPOINTS,MODEL`

- Original installer USB identified as `/dev/sda`.
- Internal NVMe target identified as `/dev/nvme0n1`.

## Deployment Incident

During the initial installation attempt, the graphical installer and other
applications became unresponsive after disk partitioning had begun.

The system remained partially responsive, but shutdown produced repeated
SQUASHFS read errors, including:

- `SQUASHFS error: Unable to read page`
- `SQUASHFS error: Unable to read fragment cache entry`

These errors indicated unreliable reads from the installation media.

The deployment was halted rather than continuing from an unreliable live
environment.

## Troubleshooting and Recovery

The Linux Mint ISO stored on Cutter was verified with SHA-256:

`a081ab202cfda17f6924128dbd2de8b63518ac0531bcfe3f1a1b88097c459bd4`

The checksum matched the official Linux Mint 22.3 Cinnamon image.

A second USB flash drive, previously used for NetBSD installation media, was
repurposed for Linux Mint.

The verified ISO was written to the replacement USB using `dd`.

After writing, the installer media was physically disconnected and
reconnected, then verified byte-for-byte against the source ISO using `cmp`.

Verification result:

`USB VERIFIED: exact match`

The Lenovo was then booted from the replacement installation media and the
deployment was restarted from scratch.

## Installation

- Existing incomplete Linux installation erased.
- Linux Mint 22.3 Cinnamon installed to the internal NVMe SSD.
- Multimedia codecs installed.
- Hostname configured as `sevilla`.
- User account configured for the system owner.
- Password required at login.
- Home-directory encryption not enabled.

## Host Naming

A family naming convention was established:

- Home networks may be named for countries.
- Individual computers may be named for cities associated with that country.

This laptop was named `sevilla`.

A hostname typo discovered during commissioning (`sevillal`) was corrected
with `hostnamectl` and `/etc/hosts`.

## Post-install Configuration

- System fully updated with APT.
- Rebooted successfully into kernel `7.0.0-31-generic`.
- Driver Manager reported no additional proprietary drivers required.
- UFW firewall enabled.
- Default firewall policy verified:
  - deny incoming
  - allow outgoing
- Automatic package updates enabled.
- Automatic Cinnamon spice updates enabled.
- Automatic Flatpak updates enabled.
- Automatic removal of obsolete kernels and dependencies enabled.

## Recovery

Timeshift configured using RSYNC.

Snapshot retention:

- Daily: 3
- Boot: 2

User home directories remain excluded from Timeshift.

A manual baseline snapshot was created:

`Clean Mint 22.3 baseline - updated - drivers verified`

Timeshift status verified as OK after reboot.

## Remote Administration

Remote administration was considered during deployment.

The system owner was not comfortable with remote administration, so no
separate remote administrator account, SSH server, or remote-access service
was configured.

This decision intentionally minimizes unnecessary remote exposure and
respects the system owner's preferred support model.

## Validation

Post-install validation confirmed:

- Correct hostname: `sevilla`
- NVMe root filesystem mounted successfully
- Wi-Fi and DNS operational
- Internet connectivity successful
- Updated kernel booted successfully
- No additional proprietary drivers required
- UFW active
- Timeshift operational with baseline snapshot present
- System reboot completed successfully
- Normal desktop functionality verified

## Post-deployment Support

Additional end-user configuration and validation were completed after the
initial deployment.

### Wireless Printing and Scanning

- Connected the laptop to the household wireless printer.
- Configured wireless printing successfully.
- Configured wireless scanning successfully.
- Verified both print and scan functionality.

### Facebook Video Calling

- Configured Facebook as a Chromium-based web application for convenient
  desktop access.
- Verified application launch and account access.
- Completed a successful video test call.
- Confirmed camera, microphone, speakers, and browser-based video calling were
  functioning correctly.

### Gaming

Gaming configuration was discussed with the system owner but intentionally
deferred at her request.

## Result

Linux Mint 22.3 Cinnamon was successfully deployed and commissioned on the
Lenovo IdeaPad 3 15IML05.

The deployment included diagnosis of faulty installation media, cryptographic
verification of the source ISO, byte-for-byte verification of replacement
media, recovery from a partial installation, operating system configuration,
update management, firewall configuration, recovery snapshots, and final
system validation.

## Status

Complete.
