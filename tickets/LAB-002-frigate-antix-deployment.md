# LAB-002: Validate antiX Deployment on Frigate

- **Status:** Completed
- **Priority:** Medium
- **Opened:** 2026-09-08
- **Assigned to:** David Smith
- **System:** Frigate — Acer Aspire One D260
- **Current OS:** antiX-26 386-full, based on Debian 13.6 (Trixie)
- **Init system:** runit

## Objective

Validate and document Frigate’s antiX deployment, restore administrative access, repair automatic network-time synchronisation, and record a baseline suitable for future Linux administration work.

## Validated Configuration

- Hostname: `frigate`
- Desktop: Fluxbox
- Architecture: 32-bit `i686`
- Processor: Intel Atom N270, 1.60 GHz
- CPU topology: one physical core, two logical CPUs
- Memory: 988 MiB RAM
- Storage: 149 GiB ext4 root filesystem on a Hitachi hard drive
- Swap: 988 MiB file at `/swap/swap`
- Network management: ConnMan
- Time synchronisation: Chrony managed by runit
- Primary administrative user: `david`

## Acceptance Criteria

- User `david` can log in and use `sudo`.
- Frigate boots normally from its internal drive.
- Wi-Fi and package repositories are accessible.
- Chrony starts automatically under runit.
- The system clock synchronises with an NTP source.
- Package configuration is internally consistent.
- Baseline system information is documented.

## Work Log

### 2026-09-08

- Recovered access after the user password was forgotten.
- Booted through GRUB recovery with `init=/bin/bash`.
- Remounted the root filesystem read-write and reset the password for `david`.
- Synchronised pending writes, remounted the filesystem read-only, and rebooted.
- Verified that `david` could log in normally and perform administrative tasks with `sudo`.
- Identified the operating system as antiX-26 386-full, based on Debian 13.6.
- Confirmed that PID 1 is runit and the machine uses the `i686` architecture.
- Confirmed Wi-Fi connectivity through ConnMan.
- Found the system clock set to 2001 with ConnMan time updates configured as manual.
- Manually corrected the system time and wrote it to the hardware clock.
- Refreshed the APT package indexes successfully; 166 packages were reported as upgradable.
- Found Chrony installed but not running automatically under runit.
- Started Chrony manually and verified successful NTP synchronisation.
- Rebooted and confirmed that the existing SysV startup links did not start Chrony under runit.
- Located and installed the antiX package `runit-service-chrony`.
- Investigated its failed post-installation script and found that `/etc/sv/chrony` was not yet supervised.
- Activated the service by linking `/etc/sv/chrony` into `/etc/service`.
- Diagnosed a restart loop caused by the run script invoking Chrony with an incorrect `--` argument.
- Manually tested `/usr/sbin/chronyd -d -F 1` and confirmed that the daemon and NTP configuration worked.
- Changed the service run command to `exec $DAEMON -n $DAEMON_OPTS`, keeping Chrony in the foreground as required by runit.
- Started the service and confirmed that it remained running under supervision.
- Completed the interrupted package configuration with `dpkg --configure -a`.
- Ran `dpkg --audit`; its silent return confirmed that no broken or partially configured packages remained.
- Rebooted and verified that runit started Chrony automatically with a new process ID.
- Confirmed synchronisation with `chronyc tracking`; the clock was within approximately one millisecond of NTP time and leap status was normal.
- Recorded a baseline of 988 MiB RAM, a 988 MiB swap file, and a 149 GiB ext4 root filesystem with approximately 131 GiB available.

## Operational Note

The runit service file `/etc/sv/chrony/run` is owned by the `runit-service-chrony` package. A future package upgrade may overwrite the local correction from `--` to `-n`. Chrony’s status should therefore be checked after relevant upgrades.

## Remaining Work

APT reported 166 upgradable packages. Applying and validating the full operating-system upgrade is outside this ticket and should be handled as separate maintenance work.

## Resolution

Frigate’s antiX deployment was validated and documented. Administrative access was restored, networking and package repositories were verified, and automatic time synchronisation was repaired under runit. Chrony now starts automatically after reboot and maintains valid NTP synchronisation. The package database is internally consistent, and the baseline hardware, memory, storage, and swap configuration have been recorded.
