# KOY IDE — downloads

KOY IDE is an agent-native IDE: chat with your project, hand tasks to AI agents that work in isolated Git worktrees,
review every change before it lands, and choose cloud models or private local models (Ollama, LM Studio).
Made by Anh Luu Services Co. Ltd..

This repository holds the **installers and the update feed** only — the source code is private.

## Download

Get the [latest release](https://github.com/https-afliz-com/koy-ide-releases/releases/latest):

| OS | File |
|---|---|
| macOS (Apple silicon) | `KOY-IDE-<version>-arm64.dmg` |
| macOS (Intel) | `KOY-IDE-<version>-x64.dmg` |
| Windows | `KOY-IDE-<version>-x64.exe` |
| Linux | `KOY-IDE-<version>-x86_64.AppImage` or `.deb` |

Install on macOS by dragging KOY IDE into **Applications** (apps run from the disk image or Downloads can't update
themselves). On Linux, keep the AppImage in a folder you can write to, e.g. `~/Applications`.

## Updates

KOY IDE checks this repository at launch and every 4 hours, downloads updates in the background after checking free
space, and installs them when you click **Restart now** (or the next time you quit).

- **Stable** (default): tagged releases.
- **Beta**: also the nightly pre-releases built from the latest code — Settings → About & updates → Update channel.

Release notes for each version are on its release page, in [CHANGELOG.md](CHANGELOG.md), and in the app under
**Help → What's New**.

## License

KOY IDE is © 2026 Anh Luu Services Co. Ltd., licensed under the **Business Source License 1.1** — see [LICENSE](LICENSE).
It is not an Open Source license; the Change License is GPL-2.0-or-later. For commercial licensing contact
legal@afliz.com or visit https://afliz.com/legal/commercial. The same license file is included in every installer.

## Files in each release

`latest.yml`, `latest-mac.yml`, `latest-linux.yml` and the `.blockmap` files are the update feed the app reads —
you don't need to download them. GitHub also attaches “Source code (zip / tar.gz)” to every release; those archives only
contain this repository (README, LICENSE, CHANGELOG), not KOY IDE's source code.
