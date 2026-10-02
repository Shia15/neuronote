# NeuroNote Voice Interface

## Purpose

Voice provides a natural-language entry point to the NeuroNote intent system.

## Flow

Voice -> transcription -> intent extraction -> security validation -> capability -> result -> human interpretation

## Example

"Read my system status."

Intent:

{
  "domain": "system",
  "action": "read",
  "scope": "local",
  "mode": "read_only"
}

The voice interface must not directly execute terminal commands.

## Security rules

- voice is an input modality, not a privilege
- authentication remains independent of voice transcription
- sensitive actions require explicit confirmation
- destructive or privileged operations require elevated authorization
- raw audio/transcripts should not be retained longer than necessary
- secrets should never be spoken into commands or logs

## Future AETHER integration

The voice result can feed the AETHER relational state model:

Human -> Intent -> Security -> System State -> Relation Graph -> AETHER visualization
