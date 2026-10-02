# NeuroNote System Readout

## Goal

Provide a normalized, human-readable view of host and network state.

## Read-only telemetry

### Host
- operating system
- hostname
- uptime
- CPU utilization
- memory utilization
- storage utilization

### Network
- interfaces
- interface status
- IPv4 state
- IPv6 state
- default routes
- DNS configuration
- neighbor observations

### Security
- firewall status
- security events exposed by the host
- authentication/session state
- recent configuration changes

## Normalized model

{
  "system": {},
  "network": {
    "interfaces": [],
    "routes": [],
    "dns": {},
    "neighbors": []
  },
  "security": {
    "firewall": {},
    "events": []
  },
  "timestamp": ""
}

## Safety

The readout layer is observational. It does not modify system settings.

System-changing operations belong behind the Security Gate and Capability Broker.
