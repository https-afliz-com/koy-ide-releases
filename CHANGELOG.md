# KOY IDE changelog

What changed in each release of KOY IDE. Download a version from its [release page](https://github.com/https-afliz-com/koy-ide-releases/releases);
the app shows the same notes in **Help → What's New**. Nightly (Beta channel) builds list their changes on their own
release pages.

## [0.21.0] - 2026-10-08

### Added
- **Orchestrator** — the first agent in Manage Agents: it's the one you talk to in chat, works out what you need, and
  hands work to Explore, Plan, Implement, Verify and Review with "Assign to …" buttons (`/task --agent <agent> …`).
  Its routing profile picks the chat's model.
- **Run plan**: when the Orchestrator splits work into steps (e.g. Implement → Verify → Review), one button runs them as
  a chain — each step starts when the one before is done and gets its results. Also `/task plan <agent>: <goal> || …`.
- **Requirements skill (Orchestrator)**: when a request is too vague to build, the Orchestrator asks up to three short
  questions first, with quick-answer buttons. Every task from chat then gets a **refined spec** — the files to create,
  the functions with their parameters, the behaviour to check, the tests, the project's rules and what's out of scope —
  ahead of your own words, so small models build the right thing. The task card shows "Spec: 3 deliverables, 2 functions …".
- **Troubleshooter** agent and **`/troubleshoot` with no issue**: scans the open project — its test, lint, typecheck and
  build commands, merge-conflict markers, links that lead out of the project, secret files committed to Git, very
  large files — and, if anything is wrong, Explore locates it, the Troubleshooter finds the root cause (reproduce → read →
  classify → fix steps), Implement fixes it and Verify checks it; the report lands in chat. No GitHub needed
  (Manage Agents → Loops → *Scan project*). `/troubleshoot <issue>` uses the Troubleshooter too.
- **Performance tester** agent and **`/perf [what]`**: times the project's tests, build and bench / perf scripts (three runs,
  median), reads the slow paths and reports hotspots and fixes ordered by impact, with "VERDICT: OK / SLOW". Agents'
  commands now report how long they took, and a project's own `bench` / `perf` scripts are on the safe list.
- `npm run perf`: a performance test for the KOY gateway (start-up, task list and filters with 500 tasks, roster, chat,
  parallel requests, state file), with a budget for each.
- **Agent memory and skills**: every agent (and the Orchestrator) learns while you work — your preferences from chat
  ("always …", "never …", "remember …"; "forget …" removes them), the project's conventions each time it opens, and per
  agent the commands and patterns that worked and why tasks failed or were rejected. It goes into their instructions
  automatically. Stored encrypted (AES-256-GCM, key in the OS keychain) on this device, never shown in the app or
  returned by the API; nothing that looks like a credential is kept. Settings → Agent memory: on/off, counts, and
  *Forget everything*.
- **Task filter**: All / Active / Review / Done / Failed / Drafts with counts, by agent, and by text; newest first; the
  last filter is remembered. The API takes `status` (a status or group, comma-separated), `agent` and `q`.
- **Agent map** in Manage Agents: the Orchestrator and its agents, how many tasks each got, who hands results to whom
  (plan and loop steps), and who is working right now. Click an agent to open it, a hand-off to open its task.
- A tip when you plan a multi-part build with only local models: add a cloud model key (one-click buttons) or split the
  work into steps.
- `/run` confirmations offer **Always allow this exact command in this project**; `/run allowed` lists them and
  `/run forget <command>` asks again. The deny-list still applies.
- Source Control: click a changed file to see its diff against the last commit (new files show as added), with Open file.
- Explorer and Source Control color changed files like VS Code — modified (M, amber), new / untracked (U / A, green),
  deleted (D, red), renamed (R), conflicts (!) — and folders that contain changes.

### Changed
- Routing profiles are now **Auto** (KOY decides for you), **Performance** (the best results) and **Reliable** (the
  lowest cost — free local models first). Older profiles keep working and show as the closest of the three.
- Manage Agents shows each agent's introduction only (no skills or raw instructions), written for KOY — no more
  "coder-1 / tester-2" from the AI Harness.
- Chat replies appear smoothly: KOY waits for the first words ("… is thinking"), then the text flows in at a steady
  pace instead of in bursts.
- Agents that write code run the project's own tests (and lint / typecheck / build when the project has them) — also
  outside a worktree for Verify — and a code task only finishes once the tests pass.
- Agents get the project's conventions in their brief: ESM or CommonJS, TypeScript, the test framework and where tests
  go, the test command and the dependencies — so they stop writing `require()` and Jest in an ESM + node:test project.
- A test run that exits 0 but finds no tests (`# tests 0`, "No tests found") no longer counts as passing.
- Every agent the Orchestrator assigns gets **your whole message** as its spec, so short goals no longer lose the file
  names, functions and behaviour you asked for. A goal the model rewrites must keep the files you named.
- On Auto, the Orchestrator plans with the Performance profile when a cloud model is usable.
- Tasks use their agent's routing profile from Manage Agents unless you pick one (they used Balanced before).
- Agents get instructions written for KOY's tools instead of the AI Harness templates (which mention `gh`, ticket
  placeholders and other agents by name).
- Tests an agent writes must import the project's code; self-contained tests don't count.
- Approving a change whose tests failed asks for confirmation first (Source Control and `/task apply` show why).
- `/troubleshoot` asks you to sign in to GitHub before it starts.
- Task numbers count up without gaps (t-1, t-2 …).
- The Tasks list and the agent map load compact task items (`GET /tasks?view=compact`): 9× less data (1.3 MB → 140 KB
  for 500 tasks) and ~7× faster; project state is saved as compact JSON (−30 %).
- A request that builds something and mentions tests ("… add test/x.test.ts") goes to Implement, not to the read-only
  Verify agent, which couldn't write the tests.
- Tasks, chat sessions and change sets are saved in KOY's data folder on this device instead of `<project>/.koy/` —
  `git clean`, deleting `.koy/` or re-cloning no longer loses them. Older state moves over on first open.
- Tasks may also write tests, `lib/`, `bin/`, `package.json` and `README.md`, not only the files they name. A JSON
  file an agent writes must be valid (a broken `package.json` stopped every command).
- Completion checks: an Implement / Fix task that changed nothing fails, Verify must end with `VERDICT: PASS` or
  `VERDICT: FAIL` (taken from the test run when the model forgets), and Review must have read files.
- Local models: 10 minutes per call instead of 2, two retries with backoff on timeouts and busy providers, then the next
  suitable model. Each local model is loaded before an agent's first call, and parallel local runners are limited to
  what fits in 70 % of the machine's RAM (and the Settings limit).

### Removed
- **Slack** — sign-in, notifications, `koy …` channel commands and the Slack app connection. Saved Slack tokens and
  settings are deleted on the next start.
- `/editfile` — describe the change with `/task …` instead; the agent makes it and you review it.
- The AFLIZ loops and `/afliz` (pm, build, review, aiops, configure). `/troubleshoot` stays.

### Fixed
- Linux release builds: electron-builder 26 refused the executable name derived from the package (`@koy/desktop`); the
  binary is now `koy-ide`, the Debian package `koy-ide`, and Linux desktops link KOY's windows to its launcher.
- **Security:** a symlink inside a project that pointed outside it (e.g. `keys -> ~/.ssh`) let the Explorer, chat and
  agents read or write there; every path check now follows links. The built app also has a Content-Security-Policy
  (only KOY's own scripts run; the page talks only to the local gateway).
- A stray rejected promise can no longer take the gateway down, and unexpected errors are logged with secrets scrubbed.
- Leftover code from removed features (unused imports, an unused `/routing/profiles` request on every Connections view).
- macOS updates on unsigned builds: "Restart now" failed with "Could not get code signature for running application".
  KOY now downloads the update for your Mac, checks its SHA-512, swaps the app after it quits and reopens it
  (signed builds keep using macOS's updater). The app must be in a folder you can write to, such as Applications.
- **New chat** (+) switches to the new conversation right away (it used to stay on the current one until a reload).
- Chat formatting: list items with `code` or **bold** no longer break into columns; `_italic_` and `> quote` blocks render.
- A finished Explore / Plan / Verify / Review task no longer says it "changed no files — describe the task again".
- Tasks created at the same time could start before their worktree existed, so their agent couldn't run commands.
- Pressing ⌘S / Ctrl+S right after typing could skip the save, and the first keys typed into a rename / new-file box
  that had just opened could be lost.

### Security
- **Electron 44** (from 33): fixes the published Electron advisories — sandbox and context-isolation bypasses,
  cross-origin reads through custom protocols, use-after-free crashes, ASAR integrity bypass, and the AppleScript
  injection in "Move to Applications" on macOS.
- **electron-builder 26**: the bundled `electron-updater` runtime no longer leaks credentials on cross-origin redirects,
  Linux AppImages no longer search untrusted library paths, and `tar` (archive path traversal and DoS) is patched.
- Development tools: Vitest 4.1 (no more Tinypool prototype-pollution RCE or mock path traversal) and a patched
  `shell-quote` for `concurrently`. `npm audit` drops from 25 findings (5 critical, 13 high) to 10 (8 moderate, 2 low,
  none in the installed app's runtime code except Monaco's built-in DOMPurify, whose affected mode it doesn't use).

## [0.20.0] - 2026-10-08

### Added
- **Slack**: Sign in with Slack (OAuth screen in the browser), notifications to your channel, and commands — post
  `koy <command>` in the channel (`koy run the tests`, `koy /afliz build …`) and the answer is threaded under it. Only
  your own messages are obeyed. An optional bot token makes messages come from a KOY bot.
- **Feedback → Google Sheet**: Help → Send feedback… (or `/feedback`) sends the text with an anonymous client id,
  timestamp, version and OS to one feedback sheet for every install (via a Google Apps Script collector). Sign in with
  Google to create the sheet; KOY triages new rows: bugs become a GitHub issue and a drafted task, feature requests an issue.
  Installed builds send to the KOY IDE Feedback form (https://forms.gle/ZzwsAQcqPXp2CErSA) — anonymous, one-way, nothing
  runs on the owner's account; another form or an Apps Script collector can be set in Settings.
- **CI → GitHub issues**: failed jobs and warnings in CI, Release and Nightly runs open (or update) GitHub issues, and
  close them when the job passes again.
- **Troubleshooting agents**: `/troubleshoot <issue>` collects the issue and its CI logs, then Explore → Plan →
  Implement → Verify fix it (with your approval) and comment the result on the issue. New `koy-troubleshoot` issues
  are announced with the command to run; `/issues` lists them.

### Security
- Secrets are never shown: the gateway token no longer appears on the command line, DevTools are off in installed
  builds, `/run` refuses to print credential files, environment dumps or keychain entries, and command output, Slack
  messages and GitHub issues are scrubbed of tokens.

### Changed
- GitHub Actions use `actions/checkout@v5` and `actions/setup-node@v5` (Node 24); builds use Node 22.

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

[0.21.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.21.0
[0.20.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.20.0
[0.19.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.19.0
[0.18.1]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.18.1
[0.18.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.18.0
[0.17.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.17.0
[0.16.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.16.0
