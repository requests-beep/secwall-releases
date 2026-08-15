# SecWall — releases

Public release channel for **SecWall**, a self-custody crypto wallet. The application
source lives in a separate private repository; this repo carries only the signed
Android APKs and the update manifest.

[**Download the latest release →**](https://github.com/requests-beep/secwall-releases/releases/latest)

---

## Installing for the first time

You do **not** need SecWall already installed to use this page. SecWall is distributed
as an APK you install yourself — it is not on Google Play — so a first install is a
normal sideload:

1. **Download** `SecWall-<version>.apk` from the
   [latest release](https://github.com/requests-beep/secwall-releases/releases/latest).
2. **Verify it** before installing — see [Verifying a download](#verifying-a-download).
   This is the step that tells you the file is the build it claims to be.
3. **Open the file** on your phone (Downloads → tap the APK). Android will ask for
   permission to install from this source the first time:
   *Settings → Apps → Special app access → Install unknown apps* → pick the app you
   downloaded with (Chrome, Files, …) → **Allow from this source**.
4. Tap **Install**.

Requires **Android 7.0 (API 24) or newer**. The APK is `arm64`/universal and around
370 KB.

> Android may warn that the app is from an unknown developer, or Play Protect may ask
> you to scan it. That is expected for any app installed outside the Play Store and is
> not a statement about this build. What actually establishes authenticity is the
> certificate check below — do that instead of relying on the warning.

Nothing is created until you choose to: on first open SecWall shows a disclaimer, then
you create or restore a wallet. It has no account, no telemetry, and no server holding
anything of yours.

---

## Updating an existing install

If SecWall is **already on your phone**, install the new APK over it the normal way —
Android replaces the app and keeps your wallets, provided the new build is signed with
the same key as the one you have.

**One exception, and it is important:** the signing key was replaced at **v1.0.20**.

<details>
<summary><strong>⚠ Upgrading from v1.0.19 or earlier — read this first</strong></summary>

The original release signing key was **lost**. v1.0.20 and later are signed with a
**new** key, and Android refuses to install an update signed by a different key.

**You cannot install v1.0.20+ on top of v1.0.19 or earlier. You have to uninstall
first — and uninstalling deletes SecWall's private storage, including your encrypted
wallet files.**

**Do not uninstall anything yet.** First, for **every** wallet in the app:

1. Open the wallet and export its backup: **⋯ → Share**, and send the `.secwall` file
   somewhere off the phone (another device, a USB drive, your own storage).
2. Make sure you still have that wallet's **recovery phrase** written down.

A `.secwall` file can only be opened with its password. A recovery phrase needs
nothing else. **If you have neither, uninstalling destroys the wallet permanently** —
this app holds the only copy of your keys, and there is no recovery service, no
password reset, and no support that can bring them back.

Only once every wallet is exported and every recovery phrase is confirmed: uninstall,
install the current release, and restore.

**You do not have to upgrade.** Staying on v1.0.19 is safe. The lost key does not
affect a build already installed on your phone — it keeps working exactly as before,
and it still verifies against its own certificate (published below). Upgrade because
you want the fixes, not because you feel you have to.

</details>

This does not affect a **first** install, and it does not affect upgrading from
v1.0.20 or later to anything newer.

---

## Verifying a download

Do this before installing, every time — first install or upgrade.

```
# APK digest — must equal the apkSha256 published for that version
Get-FileHash -Algorithm SHA256 SecWall-1.0.22.apk

# Signing certificate — "Signer #1 certificate SHA-256 digest"
apksigner verify --print-certs SecWall-1.0.22.apk
```

### Which key signed what

| Versions | Signing certificate SHA-256 |
| --- | --- |
| **v1.0.20 and later** | `f2ecb6f1bcbde758ac35f14d67f85bcb62db554b63117e6225cf329e6deb3fe8` |
| v1.0.1 – v1.0.19 | `b666793e1a1c64a59c9686a4d6efb43c4bedd3f0ddc6bb73d5ba977b9c554b15` |

Both are genuine. Check a download against the row for **the version you actually
downloaded**. A mismatch against the correct row means the file is not the build it
claims to be: **do not install it**, whatever the source.

(Before the v1.0.20 key rotation this page said a genuine build always carries the
same certificate as every previous release. That is no longer true, and following it
now would make you reject a real release. There are two legitimate certificates and
the table above says which applies where.)

Each release also attaches a `.sha256` artifact of record containing the APK digest,
the certificate digest and the source commit the build was cut from. Per-version
digests are listed in `RELEASES.md` inside each release's notes.

---

## `update.json`

The manifest an installed app reads:

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

The in-app update check is **disabled in every shipped build so far**, up to and
including v1.0.22, until its certificate pins are captured. An installed app does not
phone home and will not notify you about a new version — check
[Releases](https://github.com/requests-beep/secwall-releases/releases) yourself.

## Security

Report vulnerabilities via the contact in the app's `SECURITY.md`. Please do not open
public issues for security matters — issues are disabled on this repo.
