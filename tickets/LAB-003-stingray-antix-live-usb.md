# LAB-003: Build Persistent antiX Live USB on Stingray

- **Status:** In Progress
- **Priority:** Medium
- **Opened:** 2026-09-08
- **Assigned to:** David Smith
- **System:** Stingray — 128 GB USB drive
- **Current OS:** Persistent antiX live system
- **Build system:** Cutter

## Objective

Create a portable antiX live environment that retains changes between boots and can be used across Task Force 27 systems for Linux practice, troubleshooting, and recovery work.

## Validated Configuration

- Removable medium: 128 GB USB drive
- Built using Cutter
- Boots into antiX from USB
- Persistence is enabled
- Saved changes remain available after reboot
- Successfully tested on Cutter

## Acceptance Criteria

- Stingray boots reliably from its USB drive.
- Persistent changes survive a shutdown and subsequent boot.
- The system operates successfully on Cutter.
- Network connectivity is tested on an approved network.
- Portability is tested on at least one additional compatible system.
- Any hardware or persistence limitations discovered during testing are documented.

## Work Log

### 2026-08-30

- Identified the target USB drive as a 128 GB device and designated it `stingray`.
- Built the persistent antiX live system using Cutter.
- Booted Stingray successfully on Cutter.
- Enabled persistence and saved the live-session changes.
- Rebooted from Stingray and verified that the saved changes remained available.
- Confirmed that Stingray could serve as a portable Linux practice and recovery environment.

## Remaining Work

- Test network connectivity using an approved guest network or personal hotspot.
- Complete a sea trial on another compatible Task Force 27 system.
- Record any machine-specific boot, driver, or persistence limitations.
- Update this ticket and change its status to Completed after the remaining acceptance criteria are met.

## Risk

Boot support and hardware compatibility may differ between systems. Stingray must not be used to bypass network policies or access systems without authorisation.

## Current Resolution

The initial build and persistence tests succeeded on Cutter. Stingray is operational as a persistent antiX live USB, but broader portability and approved-network testing remain outstanding.
