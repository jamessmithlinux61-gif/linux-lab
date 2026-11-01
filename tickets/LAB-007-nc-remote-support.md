# LAB-007 — NC Remote Support Deployment

## Objective

Build a secure unattended remote-support path for a family Windows endpoint
using Tailscale for private network transport and RustDesk for interactive
desktop support.

The design must preserve directionality: approved administration systems may
initiate support connections to the endpoint, while the supported endpoint
must not gain general initiation access back into Task Force 27 systems.

## Endpoint Baseline

- Operating system: Windows 11 Home 25H2
- Remote network transport: Tailscale
- Remote desktop/support software: RustDesk
- RustDesk direct-IP port: TCP 21118

Personal account credentials, unattended-access passwords, and private overlay
addresses are intentionally excluded from this repository.

## Tailscale Configuration

The Windows endpoint was joined to the existing Tailscale environment.

Access policy was reviewed so the supported endpoint would not receive
unrestricted initiation access to administration systems.

The resulting design permits approved administration endpoints to initiate
support traffic while preserving the intended boundary against reverse access
from the supported endpoint to Carrier, Cutter, and Sevilla.

## RustDesk Configuration

RustDesk was configured for unattended support.

Configuration included:

- Permanent unattended-access password configured
- Password value intentionally excluded from documentation
- Direct-IP access enabled
- TCP port `21118` used for direct connectivity
- LAN discovery disabled
- IP whitelist restricted to the approved Carrier Tailscale source
- Unnecessary permissions disabled
- Unattended-window restriction disabled where required for support operation

## Connectivity Verification

Carrier verified direct network reachability to RustDesk on TCP port `21118`
through Tailscale.

A full unattended RustDesk session was then established from Carrier.

Validation included:

- Successful permanent-password authentication
- Remote desktop display
- Mouse control
- Keyboard input
- Successful text entry in Notepad

This confirmed that the Tailscale transport and RustDesk direct-IP path were
functional end to end.

## Security Model

The deployment follows a one-way administration model:

```text
approved administration endpoint  --->  supported Windows endpoint
supported Windows endpoint         -X->  TF27 administration systems
```

The support endpoint is not intended to function as a general-purpose peer with
Carrier, Cutter, or Sevilla.

Remote-support connectivity should be limited to the services and directions
required for support.

## Remaining Work

- Complete Cutter-side Tailscale/RustDesk client setup for support operations.
- Verify unattended RustDesk access after a full Windows reboot.
- Review Windows firewall behavior for the direct-IP support path.
- Review Windows sleep and power settings so the endpoint remains reachable
  when unattended support is expected.
- Decide whether OpenSSH provides useful additional support capability; it is
  not required for the current RustDesk design.
- Perform final reverse-access tests to confirm the supported endpoint cannot
  initiate prohibited connections to administration systems.
- Document any additional NC support endpoints separately as they are added.

## Status

In progress — Carrier-to-endpoint unattended support verified; Cutter-side and
persistence/security validation remain.
