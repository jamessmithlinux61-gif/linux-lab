# Carrier Hardware Inventory

* **Hostname:** `carrier`
* **System type:** Custom-built desktop
* **Assembly date:** 2014
* **Environment:** Task Force 27
* **Operating system:** Debian 13 with LXQt
* **Memory:** 16 GB
* **Primary storage:** 500 GB PNY CS900 SSD
* **Secondary storage:** 1 TB Western Digital HDD
* **Related incident:** [INC-002: Investigate Power Instability on Carrier](../../tickets/INC-002-carrier-power-instability.md)

## Component Inventory

* **Motherboard:** MSI A78M-E45
* **CPU:** AMD A6-6400K APU with Radeon HD Graphics
* **Graphics:** Integrated AMD Radeon HD Graphics provided by the APU
* **Memory:** 16 GB DDR3-1600
  * 2 × 8 GB Crucial Ballistix Sport DIMMs
  * Part number: `BLS8G3D1609DS1S00`
  * Installed in DIMM1 and DIMM3
* **Power supply:** Corsair CX430, 430 W
  * Model: `75-001666`
  * Part number: `CP-9020046`
* **Primary storage:** PNY CS900 500 GB SSD
* **Secondary storage:** Western Digital WD10EZEX 1 TB HDD
* **Optical drive:** SATA ATAPI optical drive
* **BIOS:** MSI Click BIOS 4, version `E7721AMS V25.1`
* **BIOS build date:** 2013-12-06

## Known Hardware Conditions

* The 24-pin ATX motherboard power connector was found partially seated with its retaining latch disengaged during `INC-002`. It was fully reseated and latched on 2026-09-16.
* SATA Port 2 was found physically loose during `INC-002`. The optical drive had been connected to this port.
* The boot SSD and optical-drive SATA data cables were moved away from the affected SATA connector block.
* SATA Port 2 should not be used pending further assessment.

## Notes

Carrier is a custom-built system and therefore has no single OEM system model or system-level serial number that can be used as a complete hardware reference.

Additional hardware details, including network adapters, expansion hardware, storage health data, and any remaining unidentified components, will be added as the investigation continues.
