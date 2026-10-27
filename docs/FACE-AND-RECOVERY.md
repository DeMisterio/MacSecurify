# Face recognition, security keys and password recovery

Face recognition and USB security keys are optional. Password unlock works at Low protection without additional runtime setup. Medium and High require the configured second factor too.

## Set up face recognition

Head movement and facial landmarks use **Apple Vision, built into macOS**. Tracking needs no additional software or model download.

1. Open **MacSecurify → Security → Face recognition → Add face**. Follow the five-slide walkthrough. It explains local processing and the optional identity/liveness engine before opening the camera. If setup is needed, choose **Copy command → Open Terminal**, click Terminal, press **⌘V**, then **Return**. Copy only the command, without your username or the `%` prompt. Python is downloaded automatically if needed; downloads require an internet connection.
2. Wait for **Face engine ready:** in Terminal, then return to the walkthrough and choose **Check installation** if needed. Existing installations skip the download. The final slide has **Start face enrollment**; only this opens the camera. Allow camera access when macOS asks.
3. Look straight ahead while the white landmarks confirm tracking. The preview frames your face automatically. Follow the arrow with a gentle circular head movement, fill the guide sections, then return to your starting position and hold still.
4. The app selects three fresh frontal samples for identity and liveness checks before saving enrollment.

The installed script can also be launched directly:

```sh
/bin/zsh "/Applications/MacSecurify.app/Contents/Resources/setup-insightface.sh"
```

This command installs dependencies; it does not enroll a face or change your password. If Python is unavailable, the script downloads the official relocatable Python 3.12 build itself and verifies its published checksum. InsightFace's pretrained models are not included in the app. Review [InsightFace licensing](https://github.com/deepinsight/insightface#license), including the supplied pretrained models’ non-commercial research restrictions, before enabling the optional feature.

## Unlock and manage your face

Choose **Face** in an authentication prompt. Verification begins immediately without a webcam preview or an extra Start button. If you enable **Security → Automatic face recognition**, eligible prompts start scanning automatically. Turn that preference off to start scans manually.

Only the normalized face template is saved in the login Keychain. Camera frames remain in memory and are discarded. Replacing or removing enrollment requires authentication. Missing models, multiple faces, unclear samples, identity mismatch or failed liveness checks refuse authentication.

The preview’s automatic zoom changes presentation; security checks receive the full camera frame. If capture fails, face the camera alone, hold still and use even light in front of you. The app may explain small-face, lighting or blur problems separately.

Webcam recognition is not Apple Face ID or equivalent to hardware Touch ID. Liveness checks do not guarantee resistance to photographs, video replay or other spoofing. No measured false-acceptance rate is claimed.

## External USB FIDO2 keys

Use **Security → External FIDO2 security keys → Add security key** to install the optional local key runtime and enroll a supported USB authenticator. Follow the PIN and physical-touch instructions. The private key stays on the device; MacSecurify stores its public credential and signature counter in Keychain. PINs are not saved.

At **High** protection, unlock requires a password of at least six characters plus a biometric or an enrolled key. The app may prevent removal of your last usable second factor until you add an alternative or change the protection level. External keys are not used to replace the hardware Touch ID requirement for an MCP grant.

## Forgot your app password?

Choose **Forgot password?** in an authentication prompt. At Low protection, supported enrolled biometrics can reset the password immediately. If any protected app uses Medium or High, recovery requires the **12-hour delay**. A High-protection reset must still use a password of at least six characters.

Recovery creates a new password; it cannot retrieve the old one. It preserves protection rules and returns to authentication without opening the originally requested app or approving settings changes.

The delayed request survives app restarts and sleep, grants no access while pending, and keeps its existing deadline if requested again. Normal authentication or cancellation clears it. Anyone using the logged-in Mac can start the delay. Same-boot elapsed time is checked, but a reboot uses the saved wall-clock deadline; changing system time across reboots remains a limitation. See [security details](../SECURITY.md).
