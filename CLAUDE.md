# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Is

A [Scoop](https://scoop.sh) bucket — a collection of app manifests for the Windows Scoop package manager — for `pivoshenko`'s tools. It currently contains a single manifest: `kasetto.json`, which installs the `kasetto` CLI (binaries `kasetto.exe` and `kst.exe`) from GitHub release archives at `github.com/pivoshenko/kasetto`.

There is no application code here, and no build, lint, test, or run commands are defined anywhere in the repo. All work is editing JSON manifests.

## Manifest Structure (kasetto.json)

Each manifest follows the standard Scoop app-manifest schema:

- `version` — current released version (no `v` prefix; the `v` is added in URLs).
- `architecture.64bit` / `architecture.arm64` — per-arch `url` (release zip: `kasetto-x86_64-pc-windows-msvc.zip` / `kasetto-aarch64-pc-windows-msvc.zip`) and SHA-256 `hash`.
- `bin` — executables exposed on PATH.
- `checkver.github` — Scoop discovers new versions from GitHub releases of the upstream repo.
- `autoupdate` — URL templates using `$version`, with hashes resolved from `$baseurl/checksums.txt` published alongside each release.

## Updating a Version

A version bump means changing, in `kasetto.json`:

1. `version`
2. Both `architecture.*.url` values (embed the new `v$version` tag)
3. Both `architecture.*.hash` values — SHA-256 of each zip, taken from the `checksums.txt` asset of the corresponding GitHub release. Never guess or reuse hashes; a wrong hash breaks `scoop install` for users.

The `autoupdate` block itself only changes if the upstream artifact naming or checksum location changes.

## Conventions

- Commit messages for version bumps follow the pattern `kasetto X.Y.Z` (see `git log`); non-release changes use conventional prefixes (`docs:`, `chore:`).
- `.editorconfig`: 2-space indentation, LF line endings, final newline, max line length 120 — applies to the JSON manifests.
- PRs use `.github/PULL_REQUEST_TEMPLATE.md`. There are no CI workflows in this repo.
