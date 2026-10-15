# distribution

Published Z_SET desktop releases. The GitHub repo is `rzset/dist`. The installed apps read their update feed from `main` here, so every push to `main` reaches users.

This repo is one of several in the zset workspace, normally checked out at `~/dev/zset/distribution`. Read the workspace rules in `~/dev/zset/AGENTS.md` first. The release steps are in `~/dev/zset/Docs/client/auto-update.md`.

## What's here

- `appcast.xml` is the Sparkle and WinSparkle feed that installs up to 3.0.3 read. The macOS items carry EdDSA signatures, and the Windows items carry DSA signatures.
- `.gitignore` ignores everything except a short allow-list. Git tracks only `appcast.xml`, `README.md`, `.gitignore` and these guide files. `macos/` and `windows/` are local staging for binaries and deltas. The published binaries are attached to GitHub Releases named after the build number (for example `303`), which is where the feed URLs point.
- The new updater on the `app-update` branch adds `update.json`. After its bridge release, `appcast.xml` stays frozen and later releases go only to `update.json`.

## Releasing

- Releases come from the client's scripts: `client/scripts/release_macos.sh`, `release_windows.sh` and `publish_release.sh`. Don't edit the feeds by hand.
- Each release is one commit with a version message, such as `Release macos 3.0.3`.
- Don't push to `main` without asking.

## Don't

- Edit, re-sign or delete a published item. Installed apps may already have read it.
