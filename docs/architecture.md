# Architecture

## Purpose

This document describes the intended high-level architecture for Shooter Support. It is a working design for the MVP and a reference point for later implementation and ADRs.

## Architectural Goals

- Mobile-first experience on the web, with PWA support.
- Semi-automatic target scoring from a phone camera.
- Accurate calculation of score, shot coordinates, mean point of impact, and zeroing advice.
- Clear separation between image processing, scoring logic, persistence, and presentation.
- Private-by-default storage with controlled sharing of individual shooting series.

## System Overview

The system is split into five main parts:

- `Web app`: user interface for capture, calibration, shot review, history, and sharing.
- `API layer`: application logic for sessions, series, target templates, sharing, and authentication checks.
- `Scoring engine`: pure domain logic for geometry, scoring, grouping, mean point of impact, and zeroing advice.
- `Image processing pipeline`: semi-automatic hit suggestion from uploaded or captured target images.
- `Data store`: persistent storage for users, firearms, sessions, series, shots, templates, and share tokens.

## Proposed Stack

- Front end: Next.js with TypeScript.
- UI deployment target: mobile web/PWA.
- Persistence: PostgreSQL-backed service such as Supabase.
- Image storage: object storage backed by the chosen platform.
- Image analysis: browser-side or server-assisted processing using a proven library such as OpenCV.js where appropriate.

## Core Domain Model

- `User`: owner of data and sharing permissions.
- `Firearm`: rifle or pistol configuration, including sight click value.
- `TargetTemplate`: geometry and scoring rules for a specific paper target.
- `ShootingSession`: one range session containing multiple series.
- `ShotSeries`: one photographed target with calibration, detected candidates, confirmed shots, and results.
- `ShotCandidate`: a suggested hit point produced by image analysis.
- `Shot`: a confirmed hit point with image and physical coordinates.
- `ZeroingAdvice`: adjustment guidance derived from the mean point of impact and click value.

## Main Flows

### Capture and scoring

1. The user starts or resumes a shooting session.
2. The user captures or uploads a target image.
3. The app stores the image and calibration data.
4. The image processing pipeline suggests candidate hits.
5. The user confirms or corrects the candidates.
6. The scoring engine calculates score, group size, and mean point of impact.
7. The app displays zeroing advice when the firearm and distance are known.

### Sharing

1. The owner marks a series as shareable.
2. The system issues a read-only share token or link.
3. A viewer can open the shared series without accessing private account data.
4. The shared view exposes only the public series result set and target image.

## Service Boundaries

### Web app

Owns the user interface and interaction state. It should not embed scoring rules directly; instead it calls shared domain functions or API endpoints.

### API layer

Owns authentication, access control, persistence, and orchestration of scoring and analysis jobs. It validates that the user may read or mutate a given session or series.

### Scoring engine

Owns all deterministic calculations:

- pixel-to-target coordinate conversion
- score assignment per ring
- mean point of impact
- group size
- MOA and mrad conversion
- zeroing click suggestions

This logic should be kept pure and testable.

### Image processing pipeline

Owns candidate hit detection only. It produces suggestions with confidence values and never replaces user confirmation for the MVP.

### Data store

Owns durable storage for application records and uploaded media metadata. Image blobs themselves should live in object storage rather than the relational database.

## Data Handling

- Images are uploaded from the mobile device and stored as objects.
- The database stores references to images, calibration data, target template selection, candidate hits, confirmed shots, and computed results.
- Calculated results should be stored so that shared views and history pages do not need to recompute them on every request.
- Sensitive records remain private unless a share token or user permission explicitly grants access.

## Non-Goals For The MVP

- Fully automatic hole detection without user confirmation.
- Native iOS or Android applications.
- Offline-first sync across multiple devices.
- Support for every target discipline from day one.

## Open Implementation Decisions

- [ADR 0001: Hit detection execution model](./adr/0001-hit-detection-execution-model.md)
- [ADR 0002: Image processing library choice](./adr/0002-image-processing-library-choice.md)
- [ADR 0003: Sharing model for series](./adr/0003-sharing-model-for-series.md)
- [ADR 0004: Production platform choice](./adr/0004-production-platform-choice.md)

## Related Documents

- [MVP plan](./shooter-support-mvp-plan.md)
- [Project todo list](./todo.md)
- [Architecture Decision Records](./adr/README.md)
