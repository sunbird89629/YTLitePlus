# Repository Guidelines

## Project Structure & Module Organization
- `YTLitePlus.xm` and `Source/` hold the main tweak logic (Logos/Objective-C). Shared headers live in `YTLitePlus.h`.
- `Tweaks/` contains bundled sub-tweaks and vendored modules (e.g., `Tweaks/YTUHD`, `Tweaks/YouPiP`, `Tweaks/YTABConfig`).
- `Bundles/` and `lang/` include localized resources and preference bundles; `Extensions/` holds app extensions bundled into the IPA.
- `Tweaks/YTHeaders/` and `Tweaks/PSHeader/` are reverse-engineered headers for YouTube/iOS APIs used by the tweak.

## Build, Test, and Development Commands
- `./build.sh`: interactive build that injects into a decrypted `YouTube.ipa` or `YouTube.app` and produces a packaged IPA in `packages/`.
- `make package THEOS_PACKAGE_SCHEME=rootless IPA="/path/to/YouTube.ipa"`: non-interactive build via Theos (same as `build.sh`).
- `make clean` or `make internal-clean`: cleans build artifacts; `internal-clean` also clears the downloaded YTLite deb in `Tweaks/YTLite/`.

## Coding Style & Naming Conventions
- Language: Objective-C/Logos (`.xm`, `.x`, `.m`). Follow existing patterns in `YTLitePlus.xm` and `Source/`.
- Indentation: 4 spaces; keep braces and spacing consistent with nearby code.
- Naming: prefer descriptive class and method names; match Apple/YouTube naming in hooked classes. Use `IsEnabled("...")` for feature flags consistent with existing tweaks.

## Testing Guidelines
- No automated test suite is present. Validate changes by building an IPA and testing on-device.
- When adding features, include a quick manual test plan in the PR (e.g., “toggle `flex_enabled`, launch app, verify overlay”).

## Commit & Pull Request Guidelines
- Commit history uses short, lowercase, action-oriented messages (e.g., `updated submodules`). Keep messages concise.
- PRs should include: a summary, target YouTube version, relevant screenshots for UI changes, and any feature flags or settings touched.

## Configuration Tips
- Requires Theos (`$THEOS` set) and a decrypted YouTube IPA or app bundle. For non-jailbroken builds, use the rootless scheme and supply `IPA=...`.

## 请尽量用中文回答我的问题