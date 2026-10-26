# MacSecurify Privacy Policy

Publisher: Dmisterio  
Contact: lorsandjase@gmail.com  
Version: 2026-10-25-2

## Who provides MacSecurify

MacSecurify is published by Dmisterio. For privacy questions, support, or a request concerning information you have sent us, contact lorsandjase@gmail.com. Effective date: 25 October 2026.

## Information stored on your Mac

MacSecurify stores your selected app names, bundle identifiers and installation paths, protected website domains, security preferences, trusted agent settings, and a dated record of the policy versions you accepted in local Application Support storage. Safari blocking rules are stored in the app’s shared container. These files are not an encrypted vault. Local access sessions and agent request history are held in memory. Recovery files may contain process identifiers and executable paths to safely release apps paused by earlier versions.

## Passwords and biometrics

Your password is processed to create a random salt and PBKDF2-SHA256 verifier in your login Keychain; the password itself is not saved. Failed-attempt throttling and any pending password-recovery request are also stored there. Touch ID is handled by macOS: MacSecurify receives an authentication result and does not receive or store fingerprints. Every fingerprint enrolled on the Mac can be used when Touch ID is enabled.

## Optional camera and face recognition

The camera opens for guided face enrollment or verification after macOS camera permission. Verification starts immediately without a video preview when you choose face recognition. If you explicitly enable Automatic face recognition in Security, verification also starts when an authentication prompt opens. The preference is off by default and can be disabled. During guided enrollment, MediaPipe processes temporary low-resolution camera frames locally to track head movement. Its face geometry and frames stay in memory and are not stored. After the movement sweep, three fresh frontal samples are processed by the separate local InsightFace helper for identity and liveness. Verification also processes three fresh samples. Camera samples are discarded after analysis. MacSecurify does not save those photographs. If you enroll, a numerical face template is stored in your login Keychain until you remove or replace it in Security. This is optional webcam recognition, not Apple Face ID, and it can be spoofed. Installing the optional engine downloads third-party software and models; the download providers receive ordinary network connection information, including your IP address.

## Safari and network activity

The Safari content blocker receives domain rules; it does not collect your browsing history or page contents. Basic app and website locking has no account, analytics, advertising, or publisher-operated cloud service, and does not upload your protection list or credentials to Dmisterio. When you open a website, Safari communicates with that website under its own settings and the website’s policy. macOS may separately retain diagnostic information under your system preferences.

## Optional AI agent integration

Local MCP clients can read protection policy and access status, including protected app identifiers and website domains. A client may send information onward under its own privacy policy. Exception requests include the integration identity, resource, purpose, and requested duration. If you explicitly approve a supported network fetch with hardware Touch ID, MacSecurify connects to that HTTPS origin and returns the fetched content to the requesting integration. The destination receives normal request metadata, including your IP address. MacSecurify does not attach browser cookies. Request and denial history is bounded and kept for the current app session.

## External FIDO2 keys

Optional external USB security keys keep their private keys on the authenticator. MacSecurify stores the public credential, its name and signature counter in your login Keychain. A key PIN is sent only to the local USB authenticator through a private in-memory helper request and is not saved. The isolated FIDO2 runtime setup downloads pinned third-party packages; no online account or publisher server is used for key authentication.

## Support messages

If you email support, your email address, message, and any attachments you choose to send are processed to answer your request through Gmail. Do not send passwords, recovery secrets, or private camera samples. Support correspondence is retained while needed to resolve and follow up on your request, then deleted when no longer needed, subject to applicable legal obligations. You can request access, correction, or deletion by contacting the address above.

## Control, retention, and removal

Local settings and accepted policy versions remain until you remove their local files; uninstalling the app alone may leave Application Support and Keychain entries. Face enrollment can be removed in Security. To stop using MacSecurify, authenticate to quit, disable its Safari extension and launch-at-login setting, then remove the app. After quitting, you can remove ~/Library/Application Support/com.deni.SecureApp and the com.deni.SecureApp / master-password login Keychain item using macOS tools. Removing that item also removes the password verifier, enrolled face template, throttling, and recovery state. Optional face-engine files are under ~/Library/Application Support/SecureApp/InsightFace. Deleting data is irreversible; removing all protections first also clears the generated domain rules. Copies in your backups are controlled by your backup settings.

## Your choices and policy updates

You must acknowledge this Privacy Policy and agree to the Terms of Use before creating a password or managing setup. This acknowledgement records that you have reviewed the policy; it does not grant camera permission or require optional biometrics or agent exceptions. You can decline and quit without creating credentials. Updated policy versions require a fresh acknowledgement before setup or management can continue. Existing protection remains active while an upgrade awaits your acknowledgement. Contact support to exercise privacy rights available under the law that applies to you.
