# Install MacSecurify

## Requirements

- A Mac with an Apple silicon processor (arm64).
- macOS 13 Ventura or later.
- Safari for website blocking. Other browsers are not covered.
- A Touch ID-capable Mac or compatible keyboard for optional Touch ID and native MCP grant approval.
- A camera and a separately configured Python 3.12 face engine for optional face recognition.

The app, its Safari extension, its recovery helper, and the MCP helper are included in the DMG. Optional face/tracking and USB security-key runtimes and models are downloaded separately. The ZIP download contains the same signed application bundle; extract it and move MacSecurify.app to Applications.

## Verify your download

Each release includes `SHA256SUMS`. In Terminal, change to the folder containing the downloaded assets and compare their hashes with that file:

```sh
shasum -a 256 MacSecurify-1.6.1-macOS-arm64.dmg
shasum -a 256 MacSecurify-1.6.1-macOS-arm64.zip
```

A checksum detects transfer damage or a different file; it does not establish the publisher’s identity by itself.

## Install

1. Download `MacSecurify-1.6.1-macOS-arm64.dmg` from the [latest preview release](https://github.com/DeMisterio/MacSecurify/releases/latest).
2. Open the DMG and drag **MacSecurify** to **Applications**.
3. Eject the disk image and open **MacSecurify** from Applications.
4. Review and acknowledge the Privacy Policy and Terms of Use, create your app password, then add applications or websites to protect.
5. For website blocking, open **Safari → Settings → Extensions**, enable **MacSecurify Blocker**, and return to MacSecurify to refresh the extension status.

Keep one installed copy so Safari and MCP clients use the same containing application. Closing the window keeps MacSecurify running in the menu bar. **Launch at login** is optional and can be enabled in settings.

### Gatekeeper and the preview

This preview is development signed and has not been notarized by Apple. macOS may refuse to open it after downloading, even when the bundle's local signature is valid. A notarized Developer ID release is needed for normal public Gatekeeper distribution.

Check that your download came from this repository's release page. If macOS blocks this preview, do not disable Gatekeeper globally or remove quarantine attributes as a troubleshooting shortcut. The release status is a known distribution limitation; include the exact macOS message in a bug report if you need help understanding it.

## Upgrading from SecureApp

1. Use **Quit SecureApp** and authenticate normally. This lets the guard release app guarding and restore website locks before it exits.
2. Install **MacSecurify** in Applications.
3. Remove the old **SecureApp.app** bundle after it has quit, keeping one installed copy of the app. Do not remove the old app's support data or Keychain items.
4. Open MacSecurify. Existing passwords, face enrollment, and protection rules are retained.
5. Check Safari's extension status and refresh it from the app. Update MCP clients to the new helper command in the [MCP guide](MCP.md).

Do not force quit the old app to perform an ordinary upgrade. If its quit prompt is open, finish authentication first.

## Everyday use

Add an installed application in **Applications**, then attempt to open it. MacSecurify brings its authentication window forward and restores access when you authenticate. Repeated attempts reuse that prompt.

Add a domain or HTTP/HTTPS URL in **Websites**. A domain rule covers its subdomains. Close or reload existing Safari tabs when adding a website: pages already rendered are outside the content blocker's new-request protection. Authenticate from MacSecurify's Websites screen to grant timed website access.

Choose 1–60 minutes of access in **Security**. **Lock all now** ends active access sessions. Quit normally when you want to stop protection.

## Troubleshooting

| Symptom | Check |
| --- | --- |
| A site still appears in Safari | Confirm the extension is enabled, return to MacSecurify and refresh its status, then reload the page. Existing page content and caches are outside this guard. |
| A website is accessible in another browser | Website blocking covers Safari only. |
| Face setup is unavailable | Install Python 3.12 and complete the separate setup. See [face recognition](FACE-AND-RECOVERY.md). |
| Touch ID is unavailable | Confirm it is configured in macOS. App unlock can use your password; an MCP capability cannot be approved without hardware Touch ID. |
| An MCP client cannot connect | Keep MacSecurify running and launch the helper from the same installed app bundle. See [MCP setup](MCP.md). |
| An agent request is refused | Check the reported reason, app mode, integration trust, and requested scope. Browser-wide unlocks are unsupported. |

See [security boundaries](../SECURITY.md) before relying on the app for a particular use case.

## Consent and Safari unlocking

Before password setup, review and acknowledge the [Privacy Policy](../PRIVACY.md) and [Terms of Use](../TERMS.md). Both are available in the app later.

The Safari setup guide appears after password creation. Enable **MacSecurify Blocker** in Safari settings, return and check the status, then add domains in **Websites**. To open a blocked domain, choose **MacSecurify → Websites → Unlock**, authenticate, and reload the original tab if necessary. The domain opens in Safari only after its rule update succeeds. Use **Setup & help** to reopen the guide.
