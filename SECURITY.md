# Security and privacy

MacSecurify is a user-space interaction guard for the current macOS account. It adds an authentication step to selected applications and Safari websites while it is running. It is not an OS sandbox, a replacement for the macOS login screen, or a boundary against an administrator or a process that already controls your account.

## Application and website protection

Application protection hides locked apps and redirects activation to authentication. Apps remain responsive, and their event loops, helpers, audio, and network work can continue. A window can briefly appear before macOS delivers an activation notification; apps that refuse to hide and other viewing routes can expose content. The current guard never sends SIGSTOP. Identity-checked recovery remains for processes paused by older versions.

The intended scope is casual physical access, not hacker attacks. `kill -9`, force termination, other low-level controls, and changes to the software or its local files can bypass the guard. See the [Terms of Use](TERMS.md) and [Privacy Policy](PRIVACY.md), which users must acknowledge before setup.

Safari protection uses a content-blocker extension for new HTTP/HTTPS requests to selected domains and their subdomains. It does not read browser history. Existing rendered pages, caches, other browsers, and Safari's user-controlled content-blocker settings are outside this protection.

Normal quit restores website locks before exiting. If restoration fails, the app refuses normal quit and retries. A crash or forced quit during temporary website access can leave that website allowed until MacSecurify restarts. Pausing protection intentionally removes Safari blocking until protection resumes.

## Credentials and biometrics

- The app password is stored as a salted PBKDF2-SHA256 verifier in the login Keychain, rather than as a retrievable password. Failed password and face attempts share persistent retry throttling.
- Touch ID is handled by macOS. MacSecurify does not read or store fingerprints. All fingerprints enrolled for the Mac can authenticate when Touch ID is enabled.
- Optional face scans run in a separately installed local InsightFace environment. Camera frames remain in memory during analysis and are discarded afterward. A normalized face template is saved in the login Keychain.
- The camera opens when face verification is selected, or automatically on an eligible authentication prompt if you enable Automatic face recognition. There is no background face monitoring. Enrollment uses separate local MediaPipe movement tracking; identity and liveness checks receive native full-frame samples.
- Optional USB FIDO2 keys retain their private key on the authenticator. High protection requires a six-character-minimum password plus a biometric or enrolled key; Medium requires password plus biometric; Low requires one method.
- Face scans require cached recognition and liveness models. Model setup uses the internet; failed setup or failed liveness checks refuse authentication.

Webcam RGB recognition is not Apple Face ID or equivalent to Touch ID's hardware protection. Matching thresholds and liveness checks are integration defaults, not measured guarantees of false-acceptance rate or replay resistance. The optional pretrained models are subject to their own non-commercial research terms. See [InsightFace licensing](https://github.com/deepinsight/insightface#license).

## Password recovery

Recovery sets a new password; it cannot reveal the previous password. Low protection allows supported enrolled biometrics or a persistent 12-hour delay. If any app uses Medium or High, the delay is required; High resets also require at least six password characters. A successful reset preserves the protection rules and returns to authentication without automatically opening the originally requested app or settings.

The delayed request survives app restarts and sleep and does not temporarily unlock protected resources. Anyone with access to the logged-in Mac can start the delay. Elapsed time is checked during the same boot, but a reboot uses the saved wall-clock deadline; changing system time across reboots remains a limitation. Successful ordinary authentication or cancellation clears a pending delay.

## MCP and agent access

The bundled stdio helper connects to the native app over private local IPC. It exposes policy and bounded request tools, not passwords, biometric templates, recovery state, settings modification, or a general unlock command. It has no TCP listener or provider API key.

The default is **Notify Only**, with exceptions disabled. **Strict** rejects exception requests. **Scoped Access** also requires explicit trust in a verified, directly launching integration. Wrapper-launched and unsigned integrations receive policy awareness without scoped grants.

Supported native approval requires fresh **hardware Touch ID**. A password, webcam scan, notification click, or synthetic UI click cannot replace that approval. The resulting capability authorizes only bounded, credential-free HTTPS GETs to the exact approved origin through the native broker. It does not grant a browser session, application-wide access, a shell command, or an exception for another process.

Capabilities are limited by duration, request count, response size, and network timeout. Expiration, revocation, relevant policy changes, screen/settings lock, adapter disconnection, and app shutdown invalidate them. Pending requests are not permission. See the [MCP guide](docs/MCP.md).

MCP is cooperative. A same-account agent with shell or Accessibility privileges can still act outside this protocol, stop a user-space guard, or manipulate its files. Code signing verifies a program's identity; it does not identify a human or prove that every instruction processed by an agent host is safe.

## Preview validation and distribution

The public preview is development signed and is not Apple notarized. Valid local signatures do not establish Gatekeeper acceptance on another Mac. The DMG is an Apple silicon build for macOS 13 or later.

Automated checks exercise authentication state, policy persistence, guarding and recovery fixtures, Safari rule behavior, MCP framing, private IPC, identity validation, subscriptions, expiry, and revocation. These checks do not establish real camera accuracy, a successful physical Touch ID interaction, Safari extension enablement on every Mac, or a complete third-party MCP client workflow. Those remain device-level validation tasks.

## Reporting a problem

For ordinary bugs, [open an issue](https://github.com/DeMisterio/MacSecurify/issues) with the app version, macOS version, and a minimal reproduction. Do not include passwords, face templates or photos, recovery data, private URLs, or personal paths.

If GitHub offers **Report a vulnerability** in this repository's Security tab, use that private channel for a sensitive report. Otherwise, contact lorsandjase@gmail.com privately without posting exploit details or personal data in a public issue.
