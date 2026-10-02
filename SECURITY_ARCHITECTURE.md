# NeuroNote Security Architecture

## Purpose

NeuroNote is evolving into a human-to-system integration layer. The security architecture separates human intent from system execution and requires sensitive actions to pass through validation and authorization.

## Core flow

Human -> Interface -> Intent -> Security Gate -> Capability -> Service -> Observation -> Interpretation -> Human

## Security principles

- Zero trust for sensitive actions
- Least privilege
- Read-only diagnostics by default
- Explicit confirmation for state-changing operations
- No arbitrary shell execution from user input
- Secrets remain server-side
- Security events are auditable
- System observations are separated from system mutation

## Core subsystems

### Identity
Establishes the authenticated user and session.

### Authorization
Determines which resources and operations the user can access.

### Intent validation
Converts natural-language requests into structured, bounded operations.

### Capability broker
Maps validated intents to an allowlisted capability.

### Audit
Records security-relevant actions without storing secrets.

### System readout
Collects read-only telemetry such as interface state, routes, DNS, firewall state, and exposed network neighbors.

### Voice
Voice input is treated as another interface to the intent layer. Speech must never become unrestricted shell input.

### Firewall
NeuroNote observes host/network firewall state and may later expose controlled configuration actions. Configuration changes require elevated authorization and explicit confirmation.

## Trust boundary

The browser/frontend must never receive or hold operating-system credentials, shell privileges, database secrets, or unrestricted command execution.

The intended boundary is:

Frontend -> authenticated API -> security gate -> approved service -> OS

## Initial implementation scope

This repository currently contains architecture and security specifications only. The application source is not present on the main branch, so these files intentionally do not claim to implement runtime behavior.
