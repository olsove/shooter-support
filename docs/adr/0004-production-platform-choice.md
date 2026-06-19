# ADR 0004: Production Platform Choice

## Status

Accepted

## Date

2026-06-20

## Context

The initial product needs authentication, PostgreSQL-backed persistence, and image storage. 
The team wants a platform that keeps the MVP operational overhead low while preserving a clear path to change later.

## Decision

Use Supabase for authentication, PostgreSQL, and object storage for the MVP.

## Alternatives Considered

- Self-hosted PostgreSQL plus custom auth and storage: maximum control, but higher setup and maintenance cost.
- A different BaaS provider: similar operational benefits, but would still introduce vendor lock-in.

## Consequences

- Faster path to implementation and deployment.
- Less infrastructure to manage during the MVP.
- The application becomes dependent on Supabase-specific operational constraints and pricing.

## Related

- [Architecture](../architecture.md)
- [MVP plan](../shooter-support-mvp-plan.md)
