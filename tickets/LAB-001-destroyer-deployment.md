# LAB-001: Deploy Minimal Debian on Destroyer

- **Status:** Completed
- **Priority:** High
- **Opened:** 2026-09-02
- **Assigned to:** David Smith
- **System:** Destroyer — Dell Latitude 2120
- **Current OS:** Debian 13.6 (Trixie), minimal installation
- **Target OS:** Debian 13.6 (Trixie), amd64 netinst

## Objective

Rebuild Destroyer as a terminal-primary Linux administration training system. The installation will use minimal Debian without a desktop environment, providing a dedicated environment for command-line practice and system administration exercises.

## Planned Configuration

- Hostname: `destroyer`
- Debian 13 stable
- No desktop environment
- Standard system utilities
- OpenSSH server
- APT/dpkg package management
- systemd
- Git
- Primary administrative user: `david`

## Deployment Plan

1. Download and verify the Debian netinst ISO.
2. Create the bootable installer USB.
3. Back up or confirm removal of any required data from Destroyer.
4. Install minimal Debian.
5. Configure networking and SSH.
6. Apply available package updates.
7. Install required administration tools.
8. Record baseline system information.
9. Verify acceptance criteria.
10. Update repository documentation and close ticket.

## Risk

Installing Debian will overwrite Destroyer’s existing antiX installation and any data stored on its internal drive.

## Rollback Plan

Reinstall antiX from existing installation media if the Debian deployment fails or proves unsuitable.

## Acceptance Criteria

- Destroyer boots from its internal drive to a text console.
- No graphical desktop environment is installed.
- User `david` can log in and perform administrative tasks with `sudo`.
- Network connectivity works.
- SSH access works from another fleet system.
- Package repositories and updates work.
- Baseline hardware and operating-system information is documented.

## Work Log

### 2026-09-02

- Downloaded `debian-13.6.0-amd64-netinst.iso`.
- Confirmed file size: `791674880` bytes.
- Verified SHA-256 checksum:
  `65273beed27b2df543b68b65630ba525cfbad8df2b12035732b2dff87d6664e7`
- Checksum matched the published value.
- Booted the verified netinst ISO through Ventoy in legacy BIOS mode.
- Replaced the existing antiX installation with minimal Debian.
- Installed standard system utilities and OpenSSH server without a desktop environment.
- Configured hostname `destroyer` and administrative user `david`.
- Locked direct password login to `root`; administrative access is provided through `sudo`.
- Partitioned the 250.1 GB internal drive using LVM:
  - Volume group: `destroyer-vg`
  - Root logical volume: approximately 91.15 GiB
  - Swap logical volume: 1.80 GiB
  - Free volume-group capacity reserved for training: 130.80 GiB
- Verified Debian 13 booted from the internal drive to a text console.
- Recorded kernel `6.12.107+deb13-amd64` and architecture `x86-64`.
- Verified Wi-Fi connectivity using DHCP address `10.0.0.249/24`.
- Verified the SSH service was active.
- Successfully connected to Destroyer over SSH from Cutter.

### 2026-09-06

- Reconnected from Cutter after DHCP changed Destroyer’s address from `10.0.0.249` to `10.0.0.247`.
- Confirmed the SSH host-key fingerprint matched the previously recorded key.
- Refreshed APT package indexes; all configured repositories responded and all packages were current.
- Searched for and inspected the `git` package before installation.
- Installed Git and verified version `2.47.3`.
- Verified systemd reported zero failed units.
- Recorded memory baseline: 1.9 GiB RAM, 269 MiB used, 1.7 GiB available, and no swap in use.
- Recorded filesystem baseline: 90 GiB root filesystem with 84 GiB available and a separate 943 MiB `/boot`.
- Verified the LVM hierarchy and confirmed 130.80 GiB remains free in `destroyer-vg` for training.
- Identified `sdb` as the empty built-in xD/SD/Memory Stick card reader.

## Resolution

Destroyer was successfully rebuilt as a minimal, terminal-primary Debian 13 system. Local administration, package management, LVM storage, networking, systemd service health, Git, and remote SSH access from Cutter were verified. All acceptance criteria were met.
