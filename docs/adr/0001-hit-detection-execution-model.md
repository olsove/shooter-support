# ADR 0001: Hit Detection Execution Model

## Status

Accepted

## Date

2026-06-20

## Context

Shooter Support needs semi-automatic hit suggestion from paper target photos in a mobile web/PWA workflow. The implementation has to balance latency, device capability, privacy, and operational complexity.

The main options are browser-only processing, server-only processing, or a hybrid approach.

## Decision

Use a hybrid hit-detection pipeline for the MVP.

The browser handles lightweight capture-time preprocessing and user interaction feedback. The API layer can perform heavier candidate generation or verification when the browser path is insufficient.

## Alternatives Considered

- Browser-only processing: simplest privacy story, but more limited on low-end devices and harder to keep consistent.
- Server-only processing: easier to centralise, but adds upload latency and increases backend workload.

## Consequences

- Better mobile responsiveness for capture and calibration.
- The pipeline is more complex to implement and test.
- Clear fallback behaviour is needed when browser-side processing fails or is unavailable.

## Related

- [Architecture](../architecture.md)
- [MVP plan](../shooter-support-mvp-plan.md)
