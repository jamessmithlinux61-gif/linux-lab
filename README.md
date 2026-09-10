# Linux-Lab — Task Force 27

A hands-on Linux and system administration learning laboratory.

This repository documents my progress toward professional Linux system administration through practical work on real hardware. Rather than treating the lab as a collection of isolated tutorials, I use the systems in **Task Force 27** to practise installation, configuration, networking, troubleshooting, documentation, and change management.

My goal is to build both the technical skills and the working habits expected of a Linux administrator.

## Current Focus

**Objective: October26**

Current study and lab work centres on:

* Linux command-line administration
* Filesystems and storage
* Users, groups, ownership, and permissions
* Package management with APT/dpkg
* systemd and service management
* Networking and network troubleshooting
* SSH and remote administration
* Git and GitHub
* Bash and administrative scripting
* Troubleshooting methodology
* Security fundamentals

Longer-term objectives include RHCSA-level Linux administration, networking, automation, and security.

## Task Force 27

Task Force 27 is a mixed-hardware home lab built around several machines with different roles and capabilities.

| System        | Platform               | Operating System           | Primary Role                                          |
| ------------- | ---------------------- | -------------------------- | ----------------------------------------------------- |
| **Carrier**   | Self-built AMD desktop | Debian 13 / LXQt           | Infrastructure host and planned central storage       |
| **Cruiser**   | Acer Aspire 5          | Windows 11                 | Windows workstation and cross-platform administration |
| **Cutter**    | Lenovo laptop          | Debian 13 / LXQt           | Primary Linux workstation and administration console  |
| **Corvette**  | Acer Chromebook 311    | ChromeOS / Debian Crostini | Portable administration endpoint                      |
| **Destroyer** | Dell Latitude 2120     | Minimal Debian 13          | Terminal-primary Linux administration server          |
| **Frigate**   | Acer Aspire One        | antiX 26 / Fluxbox         | Low-resource and legacy Linux system                  |
| **Stingray**  | 128 GB USB             | Persistent antiX Live      | Portable Linux, testing, and recovery environment     |

Detailed specifications and system status are maintained in [`inventory/systems.md`](inventory/systems.md).

## Selected Lab Work

### LAB-001 — Minimal Debian Deployment on Destroyer

Rebuilt a Dell Latitude 2120 as a terminal-primary Debian administration system.

Work included:

* Debian 13 minimal installation
* LVM disk configuration
* OpenSSH configuration and verification
* Remote administration from Cutter
* APT/dpkg package management
* Git installation
* systemd service verification
* Baseline storage, memory, networking, and system documentation

See [`tickets/LAB-001-destroyer-deployment.md`](tickets/LAB-001-destroyer-deployment.md).

### LAB-002 — antiX Validation and Service Troubleshooting on Frigate

Validated and repaired an antiX installation on a low-resource Acer Aspire One.

Work included:

* Administrative account recovery
* Wi-Fi and package-repository validation
* Investigation of incorrect system time
* Chrony/NTP troubleshooting
* runit service configuration
* Diagnosis and correction of a faulty service launch command
* Package-state validation with `dpkg`

See [`tickets/LAB-002-frigate-antix-deployment.md`](tickets/LAB-002-frigate-antix-deployment.md).

### LAB-003 — Persistent antiX Live USB

Built **Stingray**, a persistent antiX live environment intended for portable Linux practice, hardware testing, troubleshooting, and recovery work.

The initial build and persistence testing are complete. Additional cross-hardware testing remains in progress.

See [`tickets/LAB-003-stingray-antix-live-usb.md`](tickets/LAB-003-stingray-antix-live-usb.md).

### INC-001 — Unexpected Wi-Fi Connection Investigation

Investigated an unexpected connection by Cutter to an unfamiliar open wireless network.

The investigation included:

* NetworkManager profile analysis
* systemd journal and kernel-log review
* NetworkManager audit records
* Process identification
* USB and Bluetooth history
* Review of relevant network-manager behaviour
* Security-impact assessment
* Mitigation and recurrence monitoring

No evidence of malicious activity was identified.

See [`tickets/INC-001-cutter-unexpected-wifi.md`](tickets/INC-001-cutter-unexpected-wifi.md).

## Repository Structure

```text
linux-lab/
├── README.md
├── inventory/
│   └── systems.md
└── tickets/
    ├── INC-001-cutter-unexpected-wifi.md
    ├── LAB-001-destroyer-deployment.md
    ├── LAB-002-frigate-antix-deployment.md
    └── LAB-003-stingray-antix-live-usb.md
```

The repository is deliberately organised more like operational documentation than a collection of class notes.

**Inventory** records what systems exist and their current roles.

**Lab tickets** document planned deployments, configuration work, and completed projects.

**Incident tickets** document unexpected behaviour, investigation, evidence, conclusions, and corrective action.

## Working Method

Where practical, lab work follows a simple administrative workflow:

1. Define the objective.
2. Record the initial system state.
3. Plan the change or investigation.
4. Perform the work from the command line.
5. Verify the result.
6. Record problems and troubleshooting steps.
7. Document the final state.
8. Commit the work to Git.

The purpose is not merely to make something work, but to understand **why it works, how to verify it, and how to document it so another administrator could follow the reasoning**.

## Career Objective

This lab supports my transition into professional IT and Linux system administration.

I am developing practical experience in Linux administration, networking, troubleshooting, remote systems, Git-based documentation, scripting, and security while working toward RHCSA-level competence.

The repository will continue to grow as Task Force 27 gains new services, projects, incidents, and administrative responsibilities.
