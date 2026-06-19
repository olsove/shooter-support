# Shooter Support MVP Plan

## Summary

Build the first version as a mobile-friendly web app/PWA for target shooting with rifle and pistol. The MVP will let the shooter photograph a paper target, register hit points, calculate scores, find the mean point of impact, support zeroing adjustments, and save/share shooting series online.

The first version should use semi-automatic scoring: the user takes a photo, selects a target template, calibrates scale/orientation, and the system suggests likely hit points for the user to confirm or correct. This gives a better mobile workflow than purely manual scoring while keeping the technical scope narrower than fully automatic target and hole recognition.

## Key Features

- Platform: Next.js/TypeScript PWA with mobile camera support through the browser.
- Storage and sign-in: PostgreSQL-backed service such as Supabase for accounts, image storage, shooting sessions, and share links.
- Initial target type: standard circular scoring targets with known dimensions and scoring rings.
- Series workflow:
  - create a session with firearm, calibre, distance, position, ammunition and notes
  - take or upload a target photo
  - choose a target template and calibrate using the centre plus a known ring or scale
  - let the system suggest hit points from the image
  - confirm, move, add, or delete suggested hits before saving
  - calculate score, group size, mean point of impact and offset from centre
- Zeroing support:
  - show mean point of impact in mm/cm and optionally MOA/mrad
  - use the firearm's sight click value, such as `0.1 mrad` or `1/4 MOA`
  - suggest adjustment as clicks up/down and left/right
  - require confirmed distance and click value before showing advice
- Sharing:
  - private series by default
  - public read-only share link per series or session
  - shared view shows target image, hits, score, mean point of impact, distance and selected notes without exposing private profile data

## Public Interfaces and Data Model

- `User`: owner of sessions and series.
- `Firearm`: name, type `rifle | pistol`, sight system, click value and default distance.
- `TargetTemplate`: name, discipline, real-world diameter, rings and scoring zones.
- `ShootingSession`: date, location/notes, firearm, distance and series collection.
- `ShotSeries`: image, target template, calibration, detected shot candidates, confirmed shots, score, mean point of impact and sharing status.
- `ShotCandidate`: suggested image coordinate, confidence score, detection source and confirmation status.
- `Shot`: image coordinate, physical coordinate relative to centre, score and optional note.
- `ZeroingAdvice`: centre offset, unit, click value and suggested sight adjustment.

Core API operations:

- create/update firearms and click values
- create sessions and series
- upload target images
- save calibration and run hit suggestion
- save confirmed hit points
- calculate score, mean point of impact and zeroing advice server-side
- fetch private series for the owner
- fetch public shared series via share token

## Implementation Plan

1. Set up the project
   - Initialise Next.js with TypeScript, linting, tests and basic PWA configuration.
   - Add database/auth client, environment variables and a safe `.env.example`.
   - Document local commands in `README.md`.

2. Build core calculation logic
   - Implement the circular target template model.
   - Implement coordinate conversion from image pixels to real-world target coordinates.
   - Implement score calculation, mean point of impact, group size and zeroing advice.
   - Keep calculation logic in pure TypeScript modules for focused testing.

3. Build semi-automatic hit suggestion
   - Start with a browser-side image processing pipeline using canvas/OpenCV.js or a similar proven library.
   - Detect dark circular marks or changed regions inside the calibrated target area.
   - Return candidates with confidence rather than final shots.
   - Always require user confirmation before score and zeroing results are final.

4. Build mobile registration flow
   - Add screens for new session, photo capture/upload, calibration and shot review.
   - Draw target overlay, centre point, suggested hits, confirmed hits and mean point of impact over the image.
   - Support editing: confirm candidate, move shot, add shot, delete shot and reset calibration.

5. Add history and sharing
   - Add views for own sessions and series.
   - Add detailed series view with image, score, statistics and zeroing advice.
   - Add public read-only shared links.

6. Plan later extensions
   - More robust automatic hit detection from photos.
   - More target templates for specific disciplines.
   - Offline queue with later sync.
   - PDF/CSV export and progress comparison over time.

## Test Plan

- Unit tests:
  - scoring per ring
  - mean point of impact
  - image-to-target coordinate conversion
  - MOA/mrad/click calculation
  - private/public sharing rules
- Image processing tests:
  - known target images produce expected hit candidates
  - false positives can be removed before saving
  - manually added hits are included in final scoring
- Integration tests:
  - create session, upload image, save calibration, generate candidates, confirm shots and fetch result
  - public shared link works without sign-in
  - private series is not visible to other users
- Mobile UI/E2E tests:
  - camera/upload flow
  - target calibration
  - candidate review and shot editing
  - zeroing advice display
- Manual acceptance test:
  - use a known target image with manually measured hits and confirm score, mean point of impact and click advice match expectations.

## Assumptions

- MVP is a mobile web/PWA rather than a native app.
- Initial hit registration is semi-automatic: the system suggests hits, and the user confirms or corrects them.
- First supported targets are standard circular scoring targets.
- The system is for lawful sport shooting and training at a shooting range.
- Zeroing support is a mathematical aid based on registered hits, distance and click value; the user remains responsible for safe firearm handling and real-world adjustment.
