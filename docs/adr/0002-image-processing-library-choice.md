# ADR 0002: Image Processing Library Choice

## Status

Accepted

## Date

2026-06-20

## Context

The image-processing pipeline needs a proven implementation for target analysis, calibration support, and candidate hit detection. 
The chosen library must work with the planned mobile web/PWA experience.

## Decision

Use OpenCV.js where practical for browser-side image processing.

Keep the image-processing code isolated behind a narrow boundary so the implementation can be replaced or supplemented later if needed.

## Alternatives Considered

- Canvas-only custom image processing: smaller dependency surface, but too limited for robust computer-vision work.
- Server-side native OpenCV: powerful, but weaker for the browser-first MVP workflow.

## Consequences

- Uses a well-known computer-vision library with broad algorithm support.
- Increases client bundle size and may require careful performance tuning.
- Keeps future replacement options open by isolating the dependency.

## Related

- [Architecture](../architecture.md)
- [MVP plan](../shooter-support-mvp-plan.md)
