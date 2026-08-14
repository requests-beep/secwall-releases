# SecWall — releases

Public release channel for **SecWall**, a self-custody crypto wallet. The application
source lives in a separate private repository; this repo carries only the signed
Android APKs and the update manifest the installed app reads.

---

## ⚠ v1.0.20 changed the signing key — read this before you upgrade

The original release signing key was **lost**. v1.0.20 is therefore signed with a
**new** key, and Android refuses to install an update signed by a different key.

**You cannot install v1.0.20 on top of v1.0.19 or earlier. You have to uninstall
first — and uninstalling deletes SecWall's private storage, including your encrypted
wallet files.**

### If you already have SecWall installed

**Do not uninstall anything yet.** First, for **every** wallet in the app:

1. Open the wallet and export its backup: **⋯ → Share**, and send the `.secwall` file
   somewhere off the phone (another device, a USB drive, your own storage).
2. Make sure you still have that wallet's **recovery phrase** written down.

A `.secwall` file can only be opened with its password. A recovery phrase needs
nothing else. **If you have neither, uninstalling destroys the wallet permanently** —
this app holds the only copy of your keys, and there is no recovery service, no
password reset, and no support that can bring them back.

Only once every wallet is exported and every recovery phrase is confirmed: uninstall,
install v1.0.20, and restore.

### You do not have to upgrade

**Staying on v1.0.19 is safe.** The lost key does not affect a build that is already
installed on your phone — it keeps working exactly as before, and it still verifies
against its own certificate (published below). Upgrade because you want the fixes, not
because you feel you have to.

### Which key signed what

| Versions | Signing certificate SHA-256 |
| --- | --- |
| **v1.0.20 and later** | `f2ecb6f1bcbde758ac35f14d67f85bcb62db554b63117e6225cf329e6deb3fe8` |
| v1.0.1 – v1.0.19 | `b666793e1a1c64a59c9686a4d6efb43c4bedd3f0ddc6bb73d5ba977b9c554b15` |

Both are genuine. Check a download against the certificate for **the version you
actually downloaded** — see below.

[**Download v1.0.20 →**](https://github.com/requests-beep/secwall-releases/releases/latest)

---

## Verifying a download

Do this before installing, every time.

```
# APK digest — must equal the apkSha256 published for that version
Get-FileHash -Algorithm SHA256 SecWall-1.0.20.apk

# Signing certificate — "Signer #1 certificate SHA-256 digest"
apksigner verify --print-certs SecWall-1.0.20.apk
```

The certificate digest must match **the row for that version** in the table above —
`f2ecb6f1…` for v1.0.20 and later, `b666793e…` for v1.0.19 and earlier. A mismatch
against the correct row means the file is not the build it claims to be: **do not
install it**, whatever the source.

(Before the v1.0.20 key rotation this page said a genuine build always carries the
same certificate as every previous release. That is no longer true, and following it
now would make you reject a real release. There are two legitimate certificates and
the table above says which applies where.)

Each release also attaches a `.sha256` artifact of record containing the APK digest,
the certificate digest and the source commit the build was cut from.

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

`certSha256` here is the CURRENT signing certificate, so it will not match a v1.0.19
or earlier APK. Use the table above for those.

## Status

The in-app update check is **disabled in every shipped build so far**, including
v1.0.19 and v1.0.20, until its certificate pins are captured. An installed app does
not phone home and will not notify you about a new version — check
[Releases](https://github.com/requests-beep/secwall-releases/releases) yourself.

## Security

Report vulnerabilities via the contact in the app's `SECURITY.md`. Please do not open
public issues for security matters — issues are disabled on this repo.
