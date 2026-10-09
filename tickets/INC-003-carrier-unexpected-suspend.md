# INC-003: Unexpected S3 Suspend and KDE/X11 Resume Failure on Carrier

* **Status:** Resolved
* **Priority:** P2 - High
* **Incident date:** 2026-10-06
* **Resolved:** 2026-10-06
* **Assigned to:** David Smith
* **System:** [Carrier](../inventory/systems.md#carrier)
* **Operating system:** Debian 13 with KDE Plasma (X11)
* **Informal name:** Agent S3

## Summary

Carrier unexpectedly entered ACPI S3 suspend during normal use.

The system resumed when the physical power button was pressed, but the existing KDE/X11 graphical session did not recover correctly. The desktop remained visible and the mouse pointer moved, but mouse clicks and keyboard input were ignored within the graphical session.

The underlying Debian system remained responsive through a virtual console.

Journal evidence confirmed that `systemd-logind` accepted and executed a suspend request. The process or event that originated that request was not identified.

The incident was informally named **Agent S3**, referring to the unidentified actor that caused Carrier to enter ACPI S3 sleep.

## Impact

The incident interrupted active Task Force 27 administration work and rendered Carrier's graphical session unusable.

Carrier remained accessible through a local virtual console, and core operating-system services continued running.

No evidence of data loss, filesystem corruption, hardware power failure, or a full system crash was identified.

Because Carrier is intended to serve an infrastructure and administration role, unexpected suspend represents an availability risk even when the operating system resumes successfully.

## Initial Observations

Carrier had been operating continuously for several days without recurrence of the hardware power instability previously documented in [INC-002](INC-002-carrier-power-instability.md).

At approximately 09:30:

* Carrier entered full suspend.
* Case fans stopped.
* The power indicator entered its sleep-state behavior.
* Pressing the physical power button resumed the machine immediately.
* The previous graphical session reappeared without a login screen.
* The mouse pointer moved.
* Mouse clicks produced no response.
* Keyboard shortcuts within KDE produced no response.

Because the previous session resumed in place and `uptime` later confirmed continuous system uptime, the event was distinguished from a reboot or loss of power.

## Investigation

### Operating-System State

A virtual console was opened with `Ctrl+Alt+F3`.

The console remained fully responsive.

`uptime` reported continuous uptime through the event.

`systemctl is-system-running` returned:

```text
running
```

These findings established that the kernel and core operating-system environment remained functional despite the unusable graphical session.

### Suspend Confirmation

Journal review showed:

```text
systemd-logind: The system will suspend now!
```

NetworkManager then entered its normal sleep transition.

A few seconds later:

```text
systemd-sleep: Performing sleep operation 'suspend'...
kernel: PM: suspend entry (deep)
```

Carrier therefore entered the kernel's configured deep sleep mode, corresponding to ACPI S3 suspend.

On resume the kernel logged:

```text
kernel: ACPI: PM: Waking up from system sleep state S3
kernel: PM: suspend exit
```

The suspend itself was therefore an orderly system sleep transition rather than a hardware power-loss event.

### Power-Management Configuration

Carrier's KDE PowerDevil configuration contained:

```ini
[AC][SuspendAndShutdown]
AutoSuspendAction=0
```

No explicit `IdleAction`, `IdleActionSec`, or related automatic suspend policy was found in:

```text
/etc/systemd/logind.conf
/etc/systemd/logind.conf.d/
```

The available evidence therefore did not support attribution to Carrier's normal configured idle-suspend policy.

The initiating process or event remains unidentified.

### KDE/X11 Resume Failure

The graphical session continued to display the desktop and a movable pointer, but neither mouse clicks nor KDE keyboard shortcuts were accepted.

The following components remained present after resume:

* Xorg
* `kwin_x11`
* `plasmashell`
* KDE PowerDevil

No clear Radeon GPU crash was found in the journal.

The system was using:

```text
AMD/ATI Trinity 2 [Radeon HD 7540D]
Kernel driver in use: radeon
```

Radeon ring tests completed successfully during investigation.

### XInput Investigation

While troubleshooting from a virtual console, several XInput devices appeared disabled or detached from the X11 virtual core devices.

This initially suggested an input-device failure.

That interpretation was later rejected because the inspection was performed while the graphical virtual terminal was inactive. Xorg may change device state while its VT does not own the active console.

The XInput result was therefore not treated as reliable evidence of the root cause.

### KWin Recovery Attempt

`plasma-kwin_x11.service` was restarted through the user's systemd service manager.

The original KWin process failed to stop normally and eventually required termination by systemd.

The replacement KWin process then failed to complete startup successfully.

Returning to the graphical virtual terminal did not restore keyboard or mouse interaction.

### Display-Manager Recovery Attempt

SDDM was restarted.

The login screen appeared normally and accepted authentication.

After login, however, the new graphical session displayed only a blank screen with a movable mouse pointer.

`systemctl --user --failed` then showed `plasma-plasmashell.service` in a failed state.

### Plasma Shell Recovery Attempt

The failed state for `plasma-plasmashell.service` was reset and the service was started manually.

Repeated startup attempts timed out.

The active `plasmashell` process was observed waiting in a futex-related sleep state rather than completing initialization.

At this point further component-level recovery was judged unlikely to provide additional operational value.

### System Reboot

Carrier was rebooted cleanly.

Following reboot:

* KDE Plasma loaded normally.
* Mouse input worked normally.
* Keyboard input worked normally.
* Networking operated normally.
* Carrier returned to normal service.

This confirmed that the persistent failure was associated with the affected graphical-session state rather than an ongoing system-wide hardware failure.

## Secondary Finding: KDE Smart-Card PAM Profile

Immediately before suspend, `kscreenlocker_greet` logged:

```text
PAM unable to dlopen(pam_sss.so)
```

Investigation located the reference in:

```text
/usr/lib/pam.d/kde-smartcard
```

The file is owned by:

```text
libkscreenlocker6:amd64
```

The package-supplied profile contains:

```text
auth required pam_sss.so allow_missing_name try_cert_auth
```

`libpam-sss` is not installed on Carrier.

No evidence was found connecting this PAM warning to the suspend request. It was therefore classified as a secondary configuration/package observation rather than a causal finding.

The package-owned PAM file was not modified.

## Root-Cause Assessment

### Suspend Trigger

**Unresolved.**

The evidence confirms that `systemd-logind` accepted a request to suspend Carrier at approximately 09:30.

The available journal records did not identify the process or event that originated the request.

KDE automatic suspend was already configured as disabled, and no explicit systemd-logind idle-suspend policy was found.

The unidentified requester was informally designated **Agent S3**.

### Graphical Resume Failure

Carrier resumed successfully at the kernel and hardware level, but the KDE/X11 graphical session entered an unusable state.

KWin and Plasma Shell subsequently demonstrated abnormal shutdown and startup behavior during recovery attempts.

The precise internal KDE/X11 failure mechanism was not isolated.

## Corrective Action

Because Carrier is transitioning into an infrastructure role, system suspend and hibernation are not desirable during normal operation.

The following systemd targets were masked:

```text
sleep.target
suspend.target
hibernate.target
hybrid-sleep.target
suspend-then-hibernate.target
```

The change was applied with:

```bash
sudo systemctl mask \
  sleep.target \
  suspend.target \
  hibernate.target \
  hybrid-sleep.target \
  suspend-then-hibernate.target
```

Verification returned `masked` for all five targets.

Masking these targets prevents suspend and hibernate operations from being started even if another application or service requests them.

## Verification

Following corrective action:

* Carrier booted successfully.
* KDE Plasma operated normally.
* Keyboard and mouse input operated normally.
* Core services were healthy.
* KDE automatic suspend remained disabled.
* All systemd sleep and hibernate targets were verified as masked.

## Resolution

Carrier was restored to normal operation by a clean reboot.

The original suspend requester remains unidentified, but future suspend and hibernate requests are blocked at the systemd level.

The incident is considered resolved because normal service was restored and the operational risk of recurrence has been mitigated.

Carrier remains **Operational**.

## Rollback

If suspend or hibernate capability is intentionally required in the future, the containment can be removed with:

```bash
sudo systemctl unmask \
  sleep.target \
  suspend.target \
  hibernate.target \
  hybrid-sleep.target \
  suspend-then-hibernate.target
```

---

> Agent S3 remains unidentified, but no longer has access to the sleep chamber.
