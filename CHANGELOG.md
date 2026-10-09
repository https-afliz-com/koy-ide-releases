# KOY IDE changelog

The key changes in each release of KOY IDE. Download a version from its [release page](https://github.com/https-afliz-com/koy-ide-releases/releases);
the app shows the full notes in **Help → What's New**. Nightly (Beta channel) builds list their changes on their own
release pages.

## [0.23.0] - 2026-10-09

### Added

- **Pipelines** — a project's own multi-step jobs in `koy.yaml`, run with `/pipeline <name>` or `afliz pipeline <name>`
- **Releases go beta first** — the `release` pipeline pushes a beta, waits for it, downloads it like an update and smoke-tests…
- **Pause, resume and cancel local model downloads** — in Connections → Local LLM
- **Settings, redesigned** — a large window with a section list and cards that expand and collapse, each showing its current…
- **Pull requests from KOY** — `/git pr [base]` pushes the branch and opens the pull request with your GitHub sign-in
- **Smoke test** — of the packaged app on smoke data
- **Jira** — connect through the Atlassian sign-in page
- **`/git` is the whole Git command line** — log, diff, show, blame … run at once
- …and 6 smaller changes

### Changed

- **Who writes which tests** — Implement writes the unit tests with the code
- **Chat model picker** — "Auto"
- **Push, pull and fetch without signing in to GitHub** — when KOY isn't signed in, this computer's own Git setup is used, like…
- **Configure is part of AIOps** — ; `--agent configure` still works
- **Five agents, clear jobs** — the Orchestrator plans
- **Feedback goes to GitHub** — Help → Send feedback… becomes an issue in koy-ide-releases
- **Start a new project** — has a cleaner screen

### Fixed

- macOS: reopening KOY no longer shows What's New again after it was seen once
- Windows

## [0.22.0] - 2026-10-08

### Added

- **SDK release** — each release attaches `koy-sdk-<version>.tgz`
- **afliz — KOY IDE without the window** — a command line that runs the same Orchestrator and agents headless, or `afliz serve`…
- **SDK** — : a typed client for the gateway and a headless gateway you start from code
- **Start a project from Chat** — with no folder open, describe what to build
- **The Orchestrator orders requirements** — for the best result
- **Automatic retries** — a failed task runs again as a new task that is told why the last one failed
- **Numbered requirements** — : `/task` and chat split them into one small task per requirement, run one after another
- **Self-check** — before a code-writing agent finishes, it checks its work against each requirement
- …and 3 smaller changes

### Changed

- Release pages and the public changelog show each version's key points
- Agents answer faster
- Task worktrees link the project's `node_modules`, so an agent's `npm test` finds the project's own test tools

### Security

- Released builds

### Fixed

- On a machine where Git has no name and email, approving an agent's change failed
- Manage Agents

## [0.21.0] - 2026-10-08

### Added

- **Orchestrator** — the first agent in Manage Agents
- **Run plan** — when the Orchestrator splits work into steps, one button runs them as a chain
- **Requirements skill (Orchestrator)** — when a request is too vague to build, the Orchestrator asks up to three short…
- **Troubleshooter** — agent and **`/troubleshoot` with no issue**
- **Performance tester** — agent and **`/perf [what]`**
- `npm run perf`
- **Agent memory and skills** — every agent learns while you work
- **Task filter** — All / Active / Review / Done / Failed / Drafts with counts, by agent, and by text
- …and 5 smaller changes

### Changed

- Routing profiles are now **Auto**, **Performance** and **Reliable**
- Manage Agents shows each agent's introduction only, written for KOY
- Chat replies appear smoothly
- Agents that write code run the project's own tests
- Agents get the project's conventions in their brief
- A test run that exits 0 but finds no tests no longer counts as passing
- Every agent the Orchestrator assigns gets **your whole message** as its spec, so short goals no longer lose the file names,…
- On Auto, the Orchestrator plans with the Performance profile when a cloud model is usable
- …and 12 smaller changes

### Removed

- **Slack** — sign-in, notifications, `koy …` channel commands and the Slack app connection
- `/editfile`
- The AFLIZ loops and `/afliz`

### Fixed

- Linux release builds
- **Security** — a symlink inside a project that pointed outside it let the Explorer, chat and agents read or write there
- A stray rejected promise can no longer take the gateway down, and unexpected errors are logged with secrets scrubbed
- Leftover code from removed features
- macOS updates on unsigned builds
- **New chat** — switches to the new conversation right away
- Chat formatting
- A finished Explore / Plan / Verify / Review task no longer says it "changed no files
- …and 2 smaller changes

### Security

- **Electron 44** — : fixes the published Electron advisories
- **electron-builder 26** — the bundled `electron-updater` runtime no longer leaks credentials on cross-origin redirects, Linux…
- Development tools

## [0.20.0] - 2026-10-08

### Added

- **Slack** — Sign in with Slack, notifications to your channel, and commands
- **Feedback → Google Sheet** — Help → Send feedback… sends the text with an anonymous client id, timestamp, version and OS to…
- **CI → GitHub issues** — failed jobs and warnings in CI, Release and Nightly runs open GitHub issues, and close them when the…
- **Troubleshooting agents** — `/troubleshoot <issue>` collects the issue and its CI logs, then Explore → Plan → Implement →…

### Security

- Secrets are never shown

### Changed

- GitHub Actions use `actions/checkout@v5` and `actions/setup-node@v5`

## [0.19.0] - 2026-10-08

### Added

- AI Harness agents, **one per job**
- **AFLIZ loops in KOY** — the harness's `afliz` CLI as chat commands
- `npm run harness:sync` pulls the latest https-afliz-com/ai-harness from GitHub before regenerating the agent library
- License
- The public koy-ide-releases repository has its own CHANGELOG.md, refreshed automatically after every release

### Changed

- Chat `/run` runs in the open project instead of requiring a task

## [0.18.1] - 2026-10-08

### Added

- Update channels
- Nightly builds
- Releases and the update feed are published to the public `https-afliz-com/koy-ide-releases` repository, so installed apps can…

## [0.18.0] - 2026-10-08

### Added

- Code signing and notarization setup
- Release automation

### Fixed

- Chat now knows the open project
- The chat model can read, list and search project files and check Git status on its own before answering
- “Run the tests / build / lint” runs this project's own command instead of the literal words
- Local models no longer invent file contents after asking to read a file, and get a context window large enough for the project…

## [0.17.0] - 2026-10-07

### Added

- Versioned releases with this changelog
- Software updates
- **Help → Check for Updates…** — and **Help → What's New**

## [0.16.0] - 2026-10-07

### Added

- Cross-platform installers
- First-launch device check that recommends a local model size, parallel local runners and a routing profile
- DeepSeek provider
- Chat sessions in Auto routing, `/` commands and `@` file mentions
- GitHub sign-in in the default browser, organization repositories, clone and connect existing repositories

[0.23.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.23.0
[0.22.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.22.0
[0.21.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.21.0
[0.20.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.20.0
[0.19.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.19.0
[0.18.1]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.18.1
[0.18.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.18.0
[0.17.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.17.0
[0.16.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.16.0
