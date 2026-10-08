# KOY IDE changelog

What changed in each release of KOY IDE. Download a version from its [release page](https://github.com/https-afliz-com/koy-ide-releases/releases);
the app shows the same notes in **Help → What's New**. Nightly (Beta channel) builds list their changes on their own
release pages.

## [0.18.1] - 2026-10-08

### Added
- Update channels: **Stable** (tagged releases) and **Beta** (nightly builds of the latest code), in Settings → About & updates.
- Nightly builds: every night, if the code changed, GitHub Actions builds macOS, Windows and Linux and publishes a
  beta pre-release; old nightlies are pruned.
- Releases and the update feed are published to the public `https-afliz-com/koy-ide-releases` repository, so
  installed apps can update while the source stays private. `npm run github:setup` creates it and fills in both
  repositories' About information.

## [0.18.0] - 2026-10-08

### Added
- Code signing and notarization setup: hardened runtime and entitlements for macOS, Windows Authenticode through
  environment variables, `npm run signing:check`, and `docs/SIGNING.md`. Builds stay unsigned until certificates exist.
- Release automation: pushing a version tag builds, signs and uploads macOS (Apple silicon and Intel), Windows and
  Linux installers plus the update feed to GitHub Releases, then publishes the release.

### Fixed
- Chat now knows the open project: every message carries its files, stack (package.json scripts and dependencies, other
  manifests), Git branch, changes and recent commits, README, project instructions and the file open in the editor.
- The chat model can read, list and search project files and check Git status on its own before answering
  (shown as “Looked at: …”); secret files and dependencies stay out. Changes it proposes become one-click buttons.
- “Run the tests / build / lint” runs this project's own command (e.g. `npm test`) instead of the literal words.
- Local models no longer invent file contents after asking to read a file, and get a context window large enough
  for the project overview.

## [0.17.0] - 2026-10-07

### Added
- Versioned releases with this changelog: `npm run bump` sets the version in every package and dates the release notes.
- Software updates: KOY IDE checks for a new version at launch and every 4 hours, checks free space before
  downloading, notifies you when the update is ready, and installs it on **Restart now** (or the next time you quit).
- **Help → Check for Updates…** and **Help → What's New**; Settings → About shows the version, update status and
  where updates are downloaded on this computer.

## [0.16.0] - 2026-10-07

### Added
- Cross-platform installers: macOS (dmg, zip), Windows (NSIS), Linux (AppImage, deb).
- First-launch device check that recommends a local model size, parallel local runners and a routing profile.
- DeepSeek provider; parallel agent runners for cloud and local models.
- Chat sessions in Auto routing, `/` commands and `@` file mentions; mail through the OS mail app.
- GitHub sign-in in the default browser, organization repositories, clone and connect existing repositories.

[0.18.1]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.18.1
[0.18.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.18.0
[0.17.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.17.0
[0.16.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.16.0
