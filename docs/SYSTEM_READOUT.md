# NeuroNote System Readout

## Goal

Provide a normalized, human-readable view of host and network state without creating a second source of truth or modifying host networking.

## State model

The readout service is observational and should be stateless between requests unless an explicit event/history store is introduced.

- Never cache DHCP leases as authoritative state.
- Never invent an IPv4 address, route, DNS server, or interface state when the host does not report one.
- Every observation carries a timestamp and source.
- Stale observations must be marked stale rather than presented as current.
- A failed probe is different from an unavailable network resource.
- IPv4 and IPv6 are tracked independently.

## DHCP / address acquisition

NeuroNote must not act as a DHCP server or silently modify DHCP configuration.

The system readout may report:
- DHCP/automatic address configuration state when exposed by the host
- current IPv4 address, if assigned
- current IPv6 addresses
- default IPv4 route, if present
- default IPv6 route, if present
- DNS configuration
- lease/provisioning errors when exposed by the operating system

An absent IPv4 address must remain unavailable, not be converted into 0.0.0.0, a guessed address, or an inferred lease.

IPv6 availability must not be used to claim that IPv4 DHCP is healthy.

## Server-state isolation

The browser must never depend on an in-memory server variable as the authoritative representation of network state.

Use this pattern:

Host observation -> normalized response -> client display

For persistent history:

Host observation -> event record -> database -> client display

A restart must not create a false network state. If state cannot be re-observed, return unknown or stale.

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
- DHCP/automatic configuration state
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
  "system": {
    "observedAt": "",
    "source": "host"
  },
  "network": {
    "interfaces": [],
    "ipv4": {
      "state": "unknown",
      "addresses": [],
      "dhcp": "unknown"
    },
    "ipv6": {
      "state": "unknown",
      "addresses": []
    },
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

The readout layer is observational. It does not renew, release, alter, or disable DHCP.

It does not modify interfaces, routes, DNS, firewall rules, or network services.

System-changing operations belong behind the Security Gate and Capability Broker and require explicit authorization.

## Failure handling

Network failures are represented explicitly:

- unavailable: the host reports no usable state
- unknown: the observation could not be determined
- stale: a previously observed value is outside its freshness window
- reachable: the host reports a usable network path

Do not silently substitute one state for another.