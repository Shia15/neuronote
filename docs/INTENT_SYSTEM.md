# NeuroNote Intent System

## Intent model

Natural language is translated into a structured intent before any operation is selected.

Example:

"Check my network"

becomes:

{
  "domain": "network",
  "action": "diagnostic",
  "scope": "local",
  "mode": "read_only"
}

## Supported initial intent families

- system.read
- network.read
- network.neighbors
- network.routes
- network.dns
- firewall.read
- security.events
- voice.status

## Action classes

### READ
Collect state without changing the system.

### WRITE
Changes application or system state.

### EXECUTE
Runs an external capability.

### ADMIN
Changes privileged configuration.

READ is the default. WRITE, EXECUTE, and ADMIN require additional authorization.

## Command broker rule

Intent must resolve to an allowlisted capability. Arbitrary user-generated shell commands are never accepted as capabilities.

## Example

User:
"Show me the devices my computer can see."

Intent:
network.neighbors

Capability:
NETWORK_READ_NEIGHBORS

Security:
read_only = true
confirmation = false

Result:
structured network observations
