# Task Force 27 Systems Inventory

* **Owner:** David Smith
- **Lab name:** Task Force 27
* **Purpose:** Linux and IT administration training laboratory
* **Last updated:** 2026-09-08
* **Current objective:** Objective: October26

Task Force 27 is the collective name for the computers, mobile endpoints, and removable environments that make up David Smith's home Linux and IT administration laboratory.

## Computers

### Carrier

* **Platform:** Self-built AMD desktop, assembled in 2014
* **Memory:** 16 GB
* **Storage:** 500 GB PNY CS900 SSD and 1 TB Western Digital HDD
* **Operating system:** Debian 13 with LXQt
* **Role:** Primary infrastructure host and future central file-storage server
* **Capabilities:** SSH, X11 forwarding, and Tailscale
* **Status:** Maintenance required; intermittent power behaviour is awaiting investigation
* **Related tickets:** None currently recorded

### Cruiser

* **Platform:** Acer Aspire 5
* **Memory:** 12 GB
* **Operating system:** Windows 11
* **Role:** Primary Windows workstation and cross-platform administration endpoint
* **Status:** Operational
* **Related tickets:** None currently recorded

### Cutter

* **Platform:** Lenovo IdeaPad 1 14IGL05, machine type 81VU
* **Memory:** 4 GB
* **Operating system:** Debian 13 with LXQt
* **Role:** Primary Linux workstation and administrative control system
* **Capabilities:** Git, SSH, graphical administration, and removable-media preparation
* **Status:** Operational
* **Related tickets:** `INC-001-cutter-unexpected-wifi.md`

### Corvette

* **Platform:** Acer Chromebook 311 CP311-3H
* **Operating system:** ChromeOS with a Debian Crostini container
* **Role:** Portable cross-platform administration endpoint
* **Capabilities:** SSH, X11 access to Carrier, Tailscale, and LibreOffice
* **Status:** Operational
* **Related tickets:** None currently recorded

### Destroyer

* **Platform:** Dell Latitude 2120
* **Processor:** Intel Atom N550
* **Architecture:** x86-64
* **Memory:** 2 GB
* **Storage:** 250 GB internal drive using LVM
* **Operating system:** Minimal Debian 13.6 without a desktop environment
* **Role:** Terminal-primary Linux administration training server
* **Capabilities:** OpenSSH, Git, APT/dpkg, systemd, and expandable LVM storage
* **Status:** Operational
* **Related tickets:** `LAB-001-destroyer-deployment.md`

### Frigate

* **Platform:** Acer Aspire One
* **Architecture:** 32-bit x86
* **Memory:** 1 GB
* **Storage:** Approximately 140 GB internal hard drive
* **Operating system:** antiX 26 with Fluxbox
* **Role:** Low-resource Linux administration and legacy-hardware training system
* **Status:** Operational
* **Related tickets:** `LAB-002-frigate-antix-deployment.md` planned

## Mobile and Removable Assets

### Sloop

* **Platform:** Android mobile phone
* **Role:** Portable communications and remote-administration endpoint
* **Status:** Operational
* **Notes:** Hardware model and lab capabilities have not yet been recorded
* **Capabilities:** SSH client; verified remote SSH access to Carrier

### Stingray

* **Platform:** 128 GB USB flash drive
* **Operating system:** Persistent antiX live system
* **Role:** Portable Linux environment for hardware testing and recovery exercises
* **Status:** Operational
* **Related tickets:** `LAB-003-stingray-persistent-usb.md` planned

## Inventory Maintenance

Update this document whenever an asset is added, retired, repurposed, or materially reconfigured. Detailed work belongs in the associated deployment, project, maintenance, or incident ticket.
