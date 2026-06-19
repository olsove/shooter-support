# Ubiquitous Language

This document fixes the terms used across Shooter Support so the MVP, architecture, UI copy, and data model stay aligned.

## Core Terms

- `Shooter Support`: the product name.
- `User`: a signed-in person who owns shooting data.
- `Firearm`: a rifle or pistol configured in the app.
- `Shooting Session`: one range visit or practice session containing one or more series.
- `Shot Series`: one photographed target with calibration, detected candidates, confirmed shots, and computed results.
- `Shot`: a confirmed hit point on a target.
- `Shot Candidate`: a suggested hit point produced by image analysis and waiting for user confirmation.
- `Target Template`: the selected target type, including geometry, rings, and scoring rules.
- `Calibration`: the mapping between image pixels and real target distance/size.
- `Score`: the numerical result assigned to a shot or series according to the target template.
- `Group Size`: the spread of confirmed shots in a series.
- `Mean Point of Impact`: the average location of confirmed shots relative to the target centre.
- `Zeroing Advice`: the suggested sight adjustment derived from mean point of impact, distance, and click value.
- `Share Link`: a read-only link used to view a series without exposing private account data.

## Domain Rules

- A `Shot Candidate` is not a `Shot` until the user confirms it.
- A `Shot Series` belongs to exactly one `Shooting Session`.
- `Zeroing Advice` is only shown when the firearm, distance, and click value are known.
- Shared series are read-only.
- The app is private by default unless the owner explicitly shares a series.

## Measurement Terms

- `mm` and `cm`: linear distance units used for offsets and group size.
- `MOA`: minutes of angle, used for angular adjustment guidance.
- `mrad`: milliradians, used for angular adjustment guidance.
- `Click Value`: the sight adjustment represented by one click, such as `0.1 mrad` or `1/4 MOA`.

## Related Documents

- [Architecture](./architecture.md)
- [MVP plan](./shooter-support-mvp-plan.md)
- [Architecture Decision Records](./adr/README.md)
