# INC-001: Investigate Unexpected Wi-Fi Connection on Cutter

* **Status:** Monitoring
* **Priority:** Medium
* **Incident date:** 2026-09-02
* **Investigation date:** 2026-09-08
* **Assigned to:** David Smith
* **System:** Cutter — Lenovo IdeaPad 1 14IGL05
* **Operating system:** Debian 13 with LXQt

## Summary

Cutter unexpectedly connected to an unfamiliar company guest Wi-Fi network when it entered network coverage. The connection occurred without user interaction or a Wi-Fi password. Cutter had not previously been intentionally connected to this network.

Workplace-specific network identifiers have been omitted from this public-facing record.

## Impact

No confirmed security impact was identified. The event resulted in the creation of a persistent NetworkManager connection profile configured to reconnect automatically.

## Investigation

The following sources were examined:

* NetworkManager connection profiles and metadata
* systemd journal and kernel logs
* NetworkManager audit records
* Process information for the requesting application
* USB and Bluetooth device history
* Installed `nm-tray` package version
* Upstream source code for `nm-tray 0.5.1`

## Timeline

### 2026-09-02

* `07:01:08` — Cutter entered deep S3 sleep.
* `18:50:41` — Cutter resumed after firmware reported that the lid had opened.
* `18:50:41` — The keyboard controller reported an unrecognised `e078` press-and-release event during resume.
* `18:51:14` — The XScreenSaver authentication helper generated a PAM message.
* `18:52:15` — NetworkManager created and activated a profile for the company guest network.
* Shortly afterward, the same local process requested deactivation and reactivation of the connection.
* `18:53:14` — Firmware reported that the lid had closed and Cutter returned to sleep.

## Findings

* The guest-network profile was created at the time of the incident; it did not previously exist on Cutter.
* The network used no link-layer security and therefore required no Wi-Fi password.
* NetworkManager audit records attributed the create-and-activate request to PID `1319`, UID `1000`.
* PID `1319` was the legitimate `/usr/bin/nm-tray` process running in the local user session.
* Cutter had no network connection before the initial activation request, making remote initiation through that network implausible.
* No external USB input receiver was connected.
* No Bluetooth devices were paired with Cutter.
* USB reset messages were traced to Cutter’s internal webcam and Bluetooth controller.
* A disconnected SanDisk Cruzer Blade was detected during resume. Because it had been removed while Cutter was asleep, its recorded disconnection time did not establish a relationship to the wake event.
* Review of the upstream `nm-tray 0.5.1` source confirmed that its menu can create and activate an open wireless-network profile following a local GUI activation.
* Available logs identify the requesting process but do not record the physical or synthetic input event that caused the GUI activation.

## Assessment

The precise cause could not be determined retrospectively. A firmware, lid-sensor, or local input-state anomaly during resume is more consistent with the available evidence than remote compromise.

No indicators of malicious activity were found.

## Response

* Preserved the NetworkManager profile and relevant journal evidence during the investigation.
* Classified the event as isolated and inconclusive.
* Disabled automatic reconnection to the company guest network.
* Established monitoring for recurrence.

## Resolution

The investigation eliminated previously saved credentials, autonomous NetworkManager selection, remote network initiation, USB input, and paired Bluetooth input as supported explanations.

The incident is closed for active investigation and remains under observation. It should be reopened if Cutter creates or activates another unfamiliar network without deliberate user action.
