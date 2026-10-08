# KOY IDE changelog

What changed in each release of KOY IDE. Download a version from its [release page](https://github.com/https-afliz-com/koy-ide-releases/releases);
the app shows the same notes in **Help → What's New**. Nightly (Beta channel) builds list their changes on their own
release pages.

## [0.19.0] - 2026-10-08

### Added
- AI Harness agents, **one per job**: Explore, Plan, Implement, Verify and Review form the roster; Fix, Create PR,
  Requirements, Task breakdown, Configure and Notify can be added. Each uses the harness template (the AFLIZ version
  where both teams had one); duplicate templates and skills are merged. The old Planner / Coder / Tester / Security
  reviewer roles, saved profiles and "Allowed for" policies carry over to their new agents.
- **AFLIZ loops in KOY** — the harness's `afliz` CLI as chat commands: `/afliz build` (explore → plan → implement →
  verify, with fix rounds), `/afliz pm` (requirements → task list → your review), `/afliz review` (review → fix →
  review again → sign-off), `/afliz aiops` (observe → analyze → act → notify) and `/afliz configure` (model tiers),
  with the CLI's options. Every step is a normal KOY task that gets the earlier steps' results; the loop pauses at
  checkpoints and open questions (`/loop continue <id> [answers]`), posts progress to the chat, and can also be started
  from Manage Agents → Loops.
- `npm run harness:sync` pulls the latest https-afliz-com/ai-harness from GitHub (a clone next to this repository)
  before regenerating the agent library; Manage Agents shows the commit it came from.
- License: the Business Source License 1.1 from Anh Luu Services Co. Ltd. is back in the repository (it was removed
  in 0.16.0), names KOY IDE as the Licensed Work, ships inside every installer, and shows in the About panel;
  packages declare `BUSL-1.1`. `npm run github:setup` also publishes it to the koy-ide-releases repository.
- The public koy-ide-releases repository has its own CHANGELOG.md (released versions, links to their release pages),
  refreshed automatically after every release; release notes link to it and explain that GitHub's automatic
  “Source code” archives contain no KOY IDE source.

### Changed
- Chat `/run` runs in the open project instead of requiring a task: safe commands (tests, lint, build, typecheck,
  git status/diff/log, the project's own test/lint/build scripts) run at once, anything else after you confirm, and
  the deny-list (rm -rf, sudo, network tools, secrets, chaining) never runs. "Run the tests" just runs them.
  `/run @<task> <command>` still runs inside a task's worktree.

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

[0.19.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.19.0
[0.18.1]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.18.1
[0.18.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.18.0
[0.17.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.17.0
[0.16.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.16.0
