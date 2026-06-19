# ADR 0003: Sharing Model for Series

## Status

Accepted

## Date

2026-06-20

## Context

The product needs private-by-default shooting series with a way to share selected results read-only. 
Shared content must not expose private profile data or allow mutation by viewers.

## Decision

Use opaque, unguessable read-only share tokens for individual series in the MVP.

Do not introduce public profile pages or wider account-level sharing until there is a clearer requirement for them.

## Alternatives Considered

- Authenticated-only sharing: safer by default, but too restrictive for quick sharing of individual series.
- Public profile pages: easier to browse, but exposes more data than the MVP needs.

## Consequences

- Simple access model for shared views.
- Share links can be revoked by invalidating tokens.
- A later public-profile feature would need a separate decision.

## Related

- [Architecture](../architecture.md)
- [MVP plan](../shooter-support-mvp-plan.md)
