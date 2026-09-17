# INC-002: Investigate Power Instability on Carrier

* **Status:** Investigating
* **Priority:** P1 - Urgent
* **Incident date:** 2026-09-06
* **Investigation opened:** 2026-09-15
* **Assigned to:** David Smith
* **System:** [Carrier](../inventory/systems.md#carrier) — self-built AMD desktop
* **Operating system:** Debian 13 with LXQt

## Summary

Carrier has exhibited intermittent power instability, including unexpected reboots, shutdowns during startup, and abnormal power-button behavior.

The issue was first observed after Carrier was relocated within the home lab. No causal relationship between the relocation and the fault has yet been established.

Because Carrier is a custom-built system assembled in 2014, a detailed component-level hardware inventory is required as part of the investigation.

## Impact

Carrier cannot currently be considered reliable for sustained operation.

Until the fault is identified and corrected, the system should not be trusted with production-like lab services, shared fleet storage, or other workloads where an unexpected loss of power could cause data loss or service interruption.

## Risks

The unresolved power instability presents several significant risks:

* Data loss resulting from unexpected shutdowns or interrupted writes
* Filesystem corruption caused by abrupt loss of power
* Possible data loss or hardware damage associated with unstable or surge-related power conditions
* Progressive damage to storage devices or other components if the underlying electrical fault persists
* Inability to distinguish software faults from hardware faults while system power is unreliable
* Loss of availability for any services later hosted on Carrier

Because Carrier is intended to take on an infrastructure role within the lab, stable power operation is a prerequisite for further deployment work.

## Initial Observations

Observed behavior includes:

* Unexpected system reboots
* Shutdown or loss of power during startup
* Abnormal flashing of the power button
* Intermittent behavior rather than a consistent failure state

The root cause has not yet been identified.

## Immediate Response

* Classified the incident as `P1 - Urgent`.
* Deferred planned configuration changes until hardware stability is restored.
* Deferred Carrier's future file-server role until the incident is resolved.
* Identified creation of a detailed hardware inventory as an early investigation requirement.

## Investigation Plan

1. Record Carrier's motherboard, CPU, memory configuration, power supply, storage devices, expansion hardware, and other relevant components.
2. Perform an external visual inspection of power cabling, outlets, surge protection, and physical connections.
3. Inspect internal power and component connections.
4. Evaluate the power supply as a potential failure source.
5. Check system logs for evidence surrounding unexpected shutdowns and reboots.
6. Test memory, storage health, and other relevant hardware as indicated by findings.
7. Reproduce the fault under controlled conditions where practical.
8. Apply corrective action based on identified cause.
9. Perform an extended stability test before returning Carrier to normal service.
10. Document findings, corrective action, and verification results in this incident record.

## Investigation — 2026-09-16

### Physical Inspection

Carrier was inspected while powered off, disconnected from AC power, and discharged.

Initial visual inspection found no obvious scorching, melted connectors, leaking or visibly damaged motherboard components, or partially installed memory modules.

The front-panel power-switch wiring was inspected from the motherboard header to the case switch. No visible damage, pinching, or strain was observed.

The CPU power connector appeared fully seated with no visible heat damage.

A SATA data cable connected to the optical drive was observed with a pronounced bend near the motherboard connector. No exposed conductors or heat damage were visible.

### Main ATX Power Connection

During a controlled power-on test, slight upward movement of the main ATX motherboard power harness coincided with a brief flicker of the case-fan LEDs. Carrier remained powered.

The system was shut down and de-energized for inspection.

The 24-pin ATX motherboard power connector was found partially seated, with its retaining latch not engaged. The connector and motherboard socket showed no obvious scorching, melting, cracking, or heat discoloration.

The connector was fully reseated and the retaining latch was engaged.

Following reseating:

* Carrier remained powered continuously during BIOS observation.
* The system did not reproduce the previous unexpected shutdown behavior during the observation period.
* A sustained power-button press of approximately four seconds successfully powered the system down.
* Before reseating the ATX connector, a sustained press of approximately 15 seconds had failed to power the system down.

The improperly seated ATX connector is considered a significant finding and a plausible contributor to the reported power-instability symptoms. Root cause has not yet been formally confirmed pending additional stability testing.

### Boot-Device Investigation

During the initial controlled startup, Carrier completed POST but displayed:

`Reboot and Select proper Boot device or Insert Boot Media in selected Boot device and press a key`

BIOS inspection showed:

* 1 TB Western Digital WD10EZEX HDD detected
* Optical drive detected
* PNY CS900 boot SSD not detected

Further physical inspection found that the SATA socket used by the optical drive was physically loose. BIOS had identified the optical drive on SATA Port 2.

The boot SSD was connected within the same three-socket motherboard connector block.

The optical-drive and SSD SATA data cables were both relocated from that connector block to other motherboard SATA ports.

Following the cable relocation, Carrier detected the boot SSD and successfully reached the Debian login screen.

### Current Assessment

Two separate physical connection faults were identified during investigation:

1. A partially seated and unlatched 24-pin ATX motherboard power connector.
2. A physically loose motherboard SATA socket used by the optical drive.

The physically loose SATA socket is a confirmed hardware defect affecting the optical drive's original connection.

The restoration of SSD detection after both SATA data connections were moved away from the same connector block strongly associates the boot-device failure with the original SATA connection path. The specific failure affecting the SSD connection has not yet been isolated.

The improperly seated ATX connector remains the strongest candidate for the reported intermittent power-instability behavior.

The incident remains open pending extended stability testing, storage and filesystem health checks, and final verification.

### Resolution Criteria

This incident will not be considered resolved until:

* The probable root cause has been identified and corrected or otherwise mitigated.
* Carrier completes an appropriate stability-testing period without recurrence.
* Storage and filesystem health have been checked for consequences of previous abrupt power events.
* Carrier's hardware inventory has been completed and linked from the systems inventory.
* The systems inventory accurately reflects Carrier's restored operational status.

## Resolution

Pending investigation.
