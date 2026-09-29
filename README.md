**Author:** D-L3aN
# Host-Inventory-Snapshot
Add Host Inventory Snapshot recon payload  Non-destructive Windows host enumeration: identity, OS, BIOS/firmware, and installed software. Writes a plain-text report to the Desktop. Read-only, no network activity, no persistence.
## Summary

Adds a new Windows recon payload that produces a single plain-text
host inventory report on the target's Desktop. Intended for authorized
penetration tests and internal audits.

## What it collects

- **Identity** — hostname, domain/workgroup, logged-in user,
  hardware manufacturer and model
- **OS + firmware** — Windows caption, version, build, install date,
  last boot time, BIOS vendor, BIOS version, **BIOS release date**,
  and serial number
- **Software** — up to 40 installed programs (name, version, publisher)
  pulled from all three standard Uninstall registry hives, deduplicated

## What it does *not* do

- No registry, filesystem, or system configuration changes
- No persistence, scheduled tasks, services, or startup entries
- No reads of user document contents
- No network activity of any kind (no C2, no exfil, no DNS)

## Testing

| OS                | PowerShell | Result |
|-------------------|------------|--------|
| Windows 11 23H2   | 5.1        | ✅ Pass |
| Windows 10 22H2   | 5.1        | ✅ Pass |

Tested with Windows Defender enabled and default execution policy
(`Restricted`) — the payload bypasses via `-Exec Bypass` at the process
level without altering the machine's execution policy.

## Notes for maintainers

- `#REPORT_PATH` defaults to the user's Desktop for transparency.
  Operators who need covert placement can override it to `%TEMP%`.
- `#SOFTWARE_TOP 0` dumps the full software list for deep inventories.
- The payload deliberately avoids network exfiltration so it can be
  deployed in scoped engagements without additional client sign-off.

Fixes: N/A
Related: N/A
