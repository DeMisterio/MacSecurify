# Release notes

## 1.6.0 — Preview, 26 October 2026

- Stabilized automatic face framing during head movement and reserved status space to prevent the setup interface from bouncing.
- Made password, Touch ID, face, external-key and recovery prompts more compact while preserving factor requirements.
- Moved Agent MCP to the bottom of Security and added client setup guidance and terminal commands.
- Includes the HD capture, clear-frame selection, sharper preview and orbiting enrollment arrow introduced in 1.5.4.
- Distributed as a signed application ZIP and a drag-to-Applications DMG, with SHA-256 checksums.

Development-signed preview, not Apple notarized. See the installation and security guides for distribution and protection limits.

## 1.5.4 — Local build, 26 October 2026

- Added automatic face framing, a sharper preview and an orbiting arrow.
- Added native HD capture with best-frame selection and bounded full-frame detector retry at unchanged security thresholds.
- Corrected widescreen tracking geometry and clearer capture guidance.
- User confirmed enrollment and framing worked; reported preview/interface shake, addressed in 1.6.0.

## 1.5.3 — Local build, 26 October 2026

- Replaced the camera-layer preview with the exact rendered tracking frames, with an explicit Flip preview control.
- Added temporary white facial landmark dots that fade after the initial view.
- Replaced ordered direction holds with observed coverage around a circle in either direction, followed by return to neutral.

## 1.5.2 — Local build, 26 October 2026

- Replaced enrollment head-pose estimation with local MediaPipe Face Landmarker video tracking and its 3D face transform.
- Disabled preview mirroring consistently and tested camera-to-tracker row order with synthetic color quadrants.
- Added an isolated, hash-pinned tracking runtime and model; existing identity templates and InsightFace identity/liveness requirements are retained.

## 1.5.1 — Local build, 25 October 2026

- Fixed guided face setup with relative calibration, explicit continuous Vision pose detection, missing-angle fallback, and progress that tolerates small tracking noise.
- Removed JPEG encoding from movement tracking. Enrollment captures three fresh frontal samples after completing the movement sweep, retaining existing identity and liveness thresholds.
- Reduced redundant face model initialization and made canceled queued scans return promptly.

## 1.5 — Local build, 25 October 2026

- Added Low/Medium/High per-app authentication with request-bound factors and the six-character High password floor.
- Added immediate preview-free face verification, optional automatic recognition, verified head-rotation enrollment, and bounded warm model reuse.
- Added optional local USB FIDO2 key enrollment and cryptographically verified unlocking as a High second factor.
- Updated the Crow-and-shield icon on black.
- Ran external static/dependency scanners and documented confirmed findings, false positives, and scope.

This describes a local update; no public release or physical biometric/key validation is claimed.

## 1.4.1 — Local build, 25 October 2026

- Keep locked apps responsive by hiding them and redirecting activation to authentication; remove SIGSTOP-based guarding.
- Preserve recovery of processes paused by older versions.
- Explain Safari unlock → authenticate → open/reload, add extension onboarding, and expose setup help from the dashboard and menu bar.
- Add Dmisterio’s Privacy Policy and Terms of Use, two unchecked acknowledgements before setup, and a persisted version/date consent record.
- Explicitly describe casual physical-access protection, low-level bypass limitations, software modification responsibility, and mandatory legal rights.

This entry describes a local build; it does not announce a published download or completed physical-device verification.

## 1.4.0 — Earlier local preview

The MacSecurify rebrand was prepared locally; no public 1.4.0 download was published.

- Renamed the app and its public MCP helper to **MacSecurify**.
- Preserved existing local passwords, enrolled faces, and protection settings for upgrades from SecureApp.
- Includes application guarding, Safari domain rules, timed access, password and optional biometric unlock, and 12-hour delayed password recovery.
- Includes the updated Crow companion and authentication-window focus behavior for repeated attempts to open guarded apps.
- Includes a local stdio MCP adapter for policy checks, denial explanations, native request review, and revocable, bounded HTTPS GET capabilities.

### Distribution

- **System:** macOS 13 or later on Apple silicon (arm64).
- **Distribution:** this historical entry does not provide a published download.
- **Status:** development-signed preview; not Apple notarized.
- **Face recognition:** optional runtime and model downloads are separate from the DMG. Supplied pretrained models have non-commercial research restrictions.

This preview does not claim completed physical camera, Touch ID, or third-party MCP client validation. Read the [security boundaries](SECURITY.md) and [installation notes](docs/INSTALL.md).
