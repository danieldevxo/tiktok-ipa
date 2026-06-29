# tiktok-ipa

Client-side profile spoofing tweak for TikTok iOS, injected into a decrypted
IPA and built entirely in GitHub Actions (same pipeline idea as `discord-ipa`).

> **Purely local / cosmetic.** Anything spoofed here only changes what *you*
> see in your own client. Other users and TikTok's servers always see your real
> profile. Modded clients can get your account banned — use a throwaway.

## What it (aims to) spoof on your own profile

- Verified badge (blue check)
- Follower / likes counts
- Display name + @handle

## Why it's harder than Discord

Discord iOS is React Native (JS) so a JS bundle can patch anything. TikTok is a
native ObjC app with obfuscated, version-specific class/method names, so the
tweak hooks native getters on the user model (e.g. `AWEUserModel`). Method names
must be confirmed per TikTok version via `class-dump` — see the **recon**
workflow.

## Workflows

- **recon.yml** — fetch a decrypted TikTok IPA and `class-dump` it, uploading
  the headers + a summary of the relevant classes/properties as an artifact.
  Run this first to confirm the hook targets for the current TikTok version.
- **build-ipa.yml** — (added after recon) build the tweak and inject it into a
  decrypted TikTok IPA.

## Getting a decrypted IPA

The workflows accept an optional `ipa_url` pointing at a **decrypted** TikTok
IPA. If omitted they try a few community sources automatically.
