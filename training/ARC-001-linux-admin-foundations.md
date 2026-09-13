# ARC-001 — Linux Administration Foundations

## Objective

Build practical Linux administration skills through a short hands-on arc focused on package management, system and network troubleshooting, SSH administration, logging, recovery concepts, and Git-based workflow.

The emphasis was on using the terminal to investigate and solve real problems rather than following isolated command exercises.

## Work Performed

### Package and Environment Administration

* Used Debian package-management tools to install and verify administrative utilities.
* Installed packages including `tree`, `vim-runtime`, and `vim` with `apt`.
* Continued working primarily from the terminal rather than relying on graphical administration tools.

### Network and Service Troubleshooting

Investigated unexpected Wi-Fi behavior on `cutter` using system logs.

Used `journalctl` with time ranges, precise timestamps, and filtered output to examine:

* lid-close and lid-open events
* system suspend and resume
* NetworkManager state changes
* DHCP activity
* cancelled or unsuccessful lease attempts
* Wi-Fi reconnection behavior

The investigation showed the relationship between system power-state changes and NetworkManager activity and reinforced the value of logs as evidence rather than relying on assumptions.

### SSH Administration

Practiced remote administration against `carrier`.

Initial connection attempts using the hostname failed because local name resolution could not resolve `carrier`.

```text
ssh: Could not resolve hostname carrier: Name or service not known
```

Troubleshooting continued using Carrier's IP address directly.

The session included:

* accepting an SSH host key
* working through authentication failures
* successfully logging into Carrier remotely
* creating an `.ssh` directory where required
* generating an Ed25519 SSH key with `ssh-keygen`
* working with SSH configuration and key-based authentication concepts
* closing and re-establishing remote sessions from the command line

This demonstrated the distinction between network reachability, hostname resolution, host authentication, and user authentication.

### Git Integration

Practiced Git repository access using both HTTPS and SSH transport methods.

The work included:

* cloning repositories over HTTPS
* cloning repositories over SSH
* comparing the authentication and workflow differences between the two methods
* using an Ed25519 SSH key for GitHub authentication
* verifying successful SSH authentication to GitHub
* working with branches and remote repositories from the command line

This connected Linux SSH administration skills with a practical version-control workflow and reinforced the role of public/private key authentication beyond direct server logins.

### Filesystem and Recovery Troubleshooting

Worked through a boot/recovery scenario involving failure to mount a root filesystem.

Reviewed recovery methods including:

* entering maintenance/recovery conditions
* understanding `e2fsck`
* recognizing the use of alternate ext filesystem superblocks
* distinguishing filesystem-repair problems from ordinary boot or login problems

The exercise reinforced the importance of diagnosing the layer at which a failure occurs before attempting repairs.

## Skills Reinforced

This arc reinforced practical use of:

* `apt`
* `journalctl`
* `grep`
* SSH
* `ssh-keygen`
* NetworkManager logs
* DHCP troubleshooting
* filesystem-recovery concepts
* terminal-based administration
* evidence-driven troubleshooting
* Git
* GitHub
* HTTPS and SSH Git transports

## Key Takeaways

A recurring lesson throughout the arc was that similar-looking failures can occur at very different layers.

For example:

* failure to resolve a hostname is not the same as failure to reach a host
* reaching an SSH server is not the same as successfully authenticating to it
* a Wi-Fi reconnection can be understood more accurately through logs than through observation alone
* a filesystem failure during boot requires a different troubleshooting approach from a service or network failure

The work also reinforced a general administration workflow:

1. Observe the symptom.
2. Identify the subsystem involved.
3. Gather evidence.
4. Form a hypothesis.
5. Test the hypothesis.
6. Verify the result.

## Portfolio Relevance

This training arc demonstrates hands-on Linux administration across package management, logging, networking, SSH, and recovery concepts.

Rather than treating the exercises as command memorization, the focus was on diagnosing system behavior and understanding why each administrative tool was being used.
