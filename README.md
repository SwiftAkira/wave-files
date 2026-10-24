# wave-files

Temporary. Wave's own build pipeline, in a private repository, is paused
by GitHub billing. Until it runs again, this repository builds Wave on the
free runners GitHub gives public repositories, and keeps the finished apps
in its [releases](https://github.com/SwiftAkira/wave-files/releases). The
download page is still https://swiftakira.github.io/wave/.

Only the workflow is here. The code stays private, and no step prints a
key.

## What it does

`.github/workflows/ship.yml` runs every 15 minutes. It checks whether
Wave's `main` branch moved since the download page was built, and skips
changes to docs alone. When it moved, it:

1. builds the Android, Windows and Mac apps;
2. puts the three files in a new release here (the five newest are kept);
3. publishes the download page and its manifest, which point at that
   release, on the private repository's `gh-pages` branch, which GitHub
   Pages serves.

Use **Actions → Ship Wave → Run workflow** to build at once. Tick
**force** to rebuild an unchanged `main`.

## Secrets

Settings → Secrets and variables → Actions:

- `WAVE_SOURCE_TOKEN`: a fine-grained personal access token for
  `SwiftAkira/wave` only, with **Contents: Read and write**. It reads the
  code and writes the `gh-pages` branch.
- `WAVE_ANDROID_KEYSTORE`: the family signing key in base64, so the APK
  installs over the existing app.

## When the private repository's Actions run again

1. In `SwiftAkira/wave`, set Settings → Pages → Source back to
   **GitHub Actions**.
2. Re-run its newest "Build Wave for Windows and Mac" run.
3. Delete this repository and the token.
