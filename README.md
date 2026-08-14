# SecWall — releases

Public release channel for **SecWall**, a self-custody crypto wallet. The application
source lives in a separate private repository; this repo carries only the signed
Android APKs and the update manifest the installed app reads.

## `update.json`

The installed app fetches exactly one URL:

```
https://raw.githubusercontent.com/requests-beep/secwall-releases/main/update.json
```

It contains no secrets — only already-public facts about a shipped binary:

| Field | Meaning |
| --- | --- |
| `versionName` / `versionCode` | The newest published build |
| `apkSha256` | SHA-256 of the release APK |
| `certSha256` | SHA-256 of the signing certificate |
| `sourceCommit` | Commit the build was cut from |
| `sizeBytes` | Exact APK size |
| `releasePage` | Where a user is sent to download |

The app only ever compares `versionCode` against its own build. It never downloads or
installs anything: it shows a notice and opens the release page in your browser.

## Verifying a download

Do this before installing. Both values are published in `update.json` above.

```
# APK digest — must equal apkSha256
Get-FileHash -Algorithm SHA256 app-release.apk

# Signing certificate — "Signer #1 certificate SHA-256 digest" must equal certSha256
apksigner verify --print-certs app-release.apk
```

The signing certificate is the one that matters. Android refuses to install an update
signed by a different key, so a build carrying the expected `certSha256` came from the
same signing identity as every previous release. **If the certificate digest does not
match, do not install it**, regardless of where you got it.

## Status

No binary is attached yet — this repo currently publishes the manifest only, and the
in-app update check is disabled in shipped builds until its certificate pins are
captured. Releases will appear under
[Releases](https://github.com/requests-beep/secwall-releases/releases) with the signed
APK attached.

## Security

Report vulnerabilities via the contact in the app's `SECURITY.md`. Please do not open
public issues for security matters — issues are disabled on this repo.
