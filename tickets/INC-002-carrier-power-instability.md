# INC-002: Investigate Power Instability on Carrier

* **Status:** Investigating
* **Priority:** P1 - Urgent
* **Incident date:** 2026-09-06
* **Investigation opened:** 2026-09-15
* **Assigned to:** David Smith
* **System:** [Carrier](../inventory/systems.md#carrier) — self-built AMD desktop
* **Operating system:** Debian 13 with LXQt

## Summary

Carrier has exhibited intermittent power instability, including unexpected reboots, shutdowns during startup, and abnormal power-button behaviour.

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

Observed behaviour includes:

* Unexpected system reboots
* Shutdown or loss of power during startup
* Abnormal flashing of the power button
* Intermittent behaviour rather than a consistent failure state

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

## Resolution Criteria

This incident will not be considered resolved until:

* The probable root cause has been identified and corrected or otherwise mitigated.
* Carrier completes an appropriate stability-testing period without recurrence.
* Storage and filesystem health have been checked for consequences of previous abrupt power events.
* Carrier's hardware inventory has been completed and linked from the systems inventory.
* The systems inventory accurately reflects Carrier's restored operational status.

## Resolution

Pending investigation.
