---
name: shotation
description: Use when auditing frontend workflows, exporting app screenshots, checking missing UI states, preparing App Store source screenshots, or improving a codebase's screen coverage. Works for web, React Native iOS, React Native Android, native Android, and native iOS projects.
version: "0.1.0"
---

# Shotation

Shotation captures app workflows, scores frontend completeness, and produces local evidence for UI improvement.

The CLI is the npm package `shotation`; when it is not installed, run every command below as `npx shotation@latest <command>`. Composed store listings use Codex (`codex login`, ChatGPT plan) for image generation, or any key listed by `shotation listing providers`.

## Default Loop

For App Store/source screenshot work, prefer the one-command flow:

```bash
shotation app-store --base-url http://localhost:3000 --format iphone --open
```

Use multiple formats when needed:

```bash
shotation app-store --base-url http://localhost:3000 --format iphone --format ipad-13-portrait --open
```

This command auto-discovers candidate routes, captures one source size per requested output format, exports screenshots under `by-size/<width>x<height>/<format-id>/`, and records quality status in `app-store-export.json`.

For broader UI QA, use:

```bash
shotation capture --auto --base-url http://localhost:3000
shotation doctor
shotation view
```

For Android work on a fresh Mac, bootstrap first:

```bash
shotation setup android
```

Use `shotation setup android --dry-run` when you only want to inspect the install commands. The setup command checks Java, Gradle, Android SDK command-line tools, platform tools, emulator packages, and a default AVD.

Use focused reruns from Studio or from the CLI:

```bash
shotation capture --scenario home-empty --name rerun-home-empty
shotation capture --scenarios ~/scratch/app.scenarios.json --out ~/scratch/runs
```

`--scenario` selects scenarios by id or title, `--scenarios` points at a scenario file outside the
project, and `--out` writes the run somewhere other than `config.outputDir`. Together they let a
capture run against a repo without writing anything into it.

If no config exists and automatic discovery is not enough, initialize first:

```bash
shotation init --template web-ios-product
shotation capture
shotation doctor
shotation handoff
```

## Agent Responsibilities

Match the request first: audits/captures produce evidence; source changes require an implementation or improvement request. Reuse the named checkout and existing toolchain. Choose affected scenarios and required project gates; do not bootstrap Android or expand the full screenshot matrix for a small edit unless needed.

When improving a frontend:

1. Prefer real workflow scenarios.
2. Add fixture or launch-argument scenarios only for states that are slow, flaky, or impractical to reach through real UI.
3. Keep simulated screenshots clearly labeled.
4. Add missing empty, loading, error, permission, keyboard, and small-device states when the app lacks them.
5. Rerun capture after UI changes.
6. Report score movement and remaining findings.
7. For App Store exports, do not accept upscaled outputs unless the user explicitly allows them.
8. Prefer app-authored `shotation.registry.json` hooks for hard states before inventing brittle click paths.

## What To Inspect

Look for:

- Routes and navigation entries not represented in scenarios
- Lists without empty states
- Data screens without loading or error states
- Forms without validation and keyboard-open captures
- Upload or camera flows without permission-denied states
- Paywalled features without free, locked, and premium states
- Mobile views missing small-device captures
- App Store exports whose quality is `upscaled`
- Dev overlays, debug badges, blank screenshots, map tiles that did not load, and clipped UI
- Native captures whose evidence is still `simulated` instead of `real-ios-*` or `real-android-*` runtime evidence
- Android captures blocked by a permission dialog, ANR dialog, or foreground package mismatch

## App Hooks

If discovery misses important states, add `shotation.registry.json` at the app root. Use it for product-specific empty, loading, error, permission, keyboard, paywall, auth, and seed-data states.

Native targets can define build commands:

```json
{
  "build": {
    "command": "npm run ios:build",
    "outputAppPath": "ios/build/Build/Products/Debug-iphonesimulator/App.app"
  }
}
```

Android targets use APK output:

```json
{
  "build": {
    "command": "npm run android:build",
    "outputApkPath": "android/app/build/outputs/apk/debug/app-debug.apk"
  }
}
```

Swift, Expo, bare React Native, and native Android Gradle targets may also use inferred build hints from auto-discovery. Replace them when the project has a custom scheme, module, or build lane.

Set `"build": false` on a target when the app is already installed and the run should only launch it.
On iOS, Shotation pre-approves the app's URL schemes so iOS 26's `Open in "<app>"?` confirmation cannot
block a deep link, bounds every `simctl` call so a wedged simulator errors instead of hanging, and bounds
Maestro flows with `automationTimeoutMs`.

## Useful Commands

```bash
shotation templates --open
shotation init --template react-native-ios
shotation setup android
shotation list-formats
shotation app-store --base-url http://localhost:3000 --format iphone --open
shotation capture --open
shotation capture --scenario home-empty --name rerun-home-empty
shotation capture --scenarios ./scenarios.json --out ./runs
shotation export app-store
shotation view
```

## Output

Runs are written locally under `shotation-runs/` unless configured otherwise.

Important files:

- `manifest.json`
- `score.json`
- `report.md`
- `viewer/index.html`
- `prompts/agent-handoff.md`

App Store exports are written under `shotation-exports/app-store/<run-id>/` by default. Important files:

- `app-store-export.json`
- `README.md`
- `by-size/<width>x<height>/<format-id>/*.png`
