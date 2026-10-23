# KOY IDE 2 changelog

What changed in the app in each release of KOY IDE 2. Download a version from its [release page](https://github.com/https-afliz-com/koy-ide-releases/releases);
the app shows the full notes in **Help → What's New**.

## [2.5.0] - 2026-10-22

### Added

- **Afliz's shared models (Afliz employees only)** — signed in with an @afliz.com account, KOY picks up the model servers Afliz switches on in the account portal (for example GLM 4.7 Flash on Afliz's own…
- **19 languages** — Settings → Appearance → Language: English, Tiếng Việt, 日本語, 한국어, 简体中文, ไทย, Bahasa Indonesia, Bahasa Melayu, Français, Deutsch, Español, Português,…

### Fixed

- **A task paused because the computer was busy could wait forever** — on a computer that stays busy (for example a Mac with little free memory), a task paused at the moment another one finished was never picked up again — the…

### Security

- **KOY ID sign-ins must include the second step** — KOY now refuses a KOY ID ID token that doesn't say two-step verification was done (`acr` = `mfa`), when you sign in and when it renews the sign-in, and asks…

## [2.4.0] - 2026-10-17

### Added

- **Reply OK to go ahead** — chat: when the Orchestrator offers something — a plan, an "Assign to …", a command it asks to run, a skill for an agent — reply **OK** (or *yes*, *go…
- **`/skill <agent> <what it should do from now on>`** — teach one of your agents a skill for this project, from chat

### Changed

- **Sign in with KOY ID** — KOY IDE now signs in with KOY ID, Afliz's own account sign-in (id.afliz.com, with two-step verification), in your browser — no more Google / GitHub /…
- **App connector icons** — Connections → Apps: each connector shows its own icon instead of the same plug for all
- **"Share usage events"** — the privacy switch (Settings → Privacy), the welcome tour and the "Help improve KOY?" prompt now say exactly what is shared — usage events such as which…

## [2.3.0] - 2026-10-14

### Changed

- **Signing in and upgrading happen on afliz.com** — (afliz.com/account/) instead of koyide-6137b.web.app, which is now only for development builds
- **No emoji** — chat replies, notifications and the CLI use plain marks (✓ ✖ ⚠ ▶) instead of colour emoji, and agents, routing profiles, harness templates and apps carry…

### Fixed

- **Images open in the Explorer** — PNG, JPEG, GIF, WebP, SVG, BMP, ICO, AVIF, TIFF and HEIC now open in an image viewer (fit or actual size, with its dimensions and file size) instead of as…

### Added

- **Usage sharing now reaches Google Analytics** — when you choose to share anonymous usage (the welcome tour, or Settings → Privacy), KOY sends the app opening, the views you open and the names of the…

## [2.2.0] - 2026-10-13

### Fixed

- **You can move the window on a Mac again** — Drag it by the top bar (anywhere that isn't a button or the search box); a double click zooms it, as with any Mac window
- **`afliz` works through the open KOY IDE 2 again** — it looked for the running app only in KOY IDE 1.x's folders, so since 2.0 every command started a separate headless gateway with its own data — tasks,…
- **`afliz` finds the installed KOY IDE 2's gateway** — when the app isn't open, so a copy of the CLI outside this repository (e.g. in another project's `tools/`) no longer needs `koy-gateway` on the PATH or…
- **One KOY IDE 2 at a time** — opening it again brings the open window forward instead of starting a second copy — which ran its own gateway on the same data folder and took over the link…
- **`koy-gateway --version` and `--help` answer and stop** — instead of starting a gateway on your data folder; an unknown option is refused

## [2.1.1] - 2026-10-13

### Fixed

- **KOY IDE 2 for Windows updates itself** — like the Mac app: it checks at launch and every 4 hours, downloads a new version in the background and installs it on **Restart now** or the next quit…

## [2.1.0] - 2026-10-13

### Added

- **Focus mode** — View → Focus Mode, Cmd/Ctrl+Shift+F: the app turns monochrome and shows only your work — the editor, chat and the search bar (Cmd/Ctrl+K opens files)
- **Agent tasks scale with your computer's load** — (Settings → This computer, on by default)
- **MCP servers** — Settings → MCP servers: give agents extra tools from any Model Context Protocol server — one KOY starts on this computer (a command such as `npx …`) or a…
- **Use your Claude plan through Claude Code** — Connections → Model keys → Your Claude plan: agents and chat can run on Claude Pro, Max, Team or Enterprise through the `claude` CLI, with no API key

### Changed

- **The Orchestrator runs on your Claude plan while Claude Code is ready** — when Claude Code is installed and signed in on a paid plan, KOY adds it by itself (no click, no key) and chat uses it — Opus first, then Sonnet, then Haiku,…
- **Manage Agents** — no longer shows the AI Harness version and sync date under the library

### Fixed

- **The Apple silicon (arm64) Mac app opens again** — Without a Developer ID it shipped with only the compiler's signature, which macOS rejects on Apple silicon, so the Intel build was the one that ran (through…
- **The local model suggestion checks what you already have** — If a coding model that fits your computer is already installed, setup no longer ticks a download (a bigger one is offered as optional); an installed model…
- **Windows: approving a change no longer fails with a conflict that isn't there** — Git for Windows keeps files with Windows line endings; KOY now compares and merges the text itself, so a change made against an older version merges, a…
- **Windows: keys and secrets can be typed in KOY's terminal** — `afliz secret set` waited for ever after Enter, so API keys (Connections → Model keys → *Add & set key in terminal*) couldn't be added
- **Windows: Claude Code installed with npm answers** — Every call with instructions failed to start (`claude.cmd` can't take them on its command line); they now go with the prompt
- **Windows: tasks run with the KOY CLI show in the app** — The app and the CLI now find a project's data under the same folder path and home folder
- **Windows: koy.yaml pipelines run in Git Bash** — when Git for Windows is installed, so the same steps (`a && b`, `$SECRET`) work on every OS — KOY IDE's own pipelines included
- **An MCP server that stops while starting shows its exit code** — with its error output
- Messages and hints point to **Connections → App connectors** (they still said *Apps*)

## [2.0.0] - 2026-10-12

### Removed

- **The Electron app is no longer supported** — KOY IDE 1.x (Electron) gets no further updates or fixes: 1.1.0 is its last version

### Changed

- **The app is now called KOY IDE 2** — every 2.x version — so it is never mistaken for KOY IDE 1.x: a separate app (KOY IDE 2.app, its own bundle id) with its own data folder
- **A smaller, faster KOY IDE 2** — the desktop app is now built on Tauri, using your system's web view instead of a bundled browser, and its engine (the gateway behind every screen, the CLI…
- **`afliz` in KOY's terminals works without Node.js** — the commands the app asks you to run (`afliz secret set …`) and the everyday ones (`status`, `chat`, `task`, `tasks`, `show`, `approve`, `scan`,…
- **The agent team works as one** — Each agent has one job and its own files (docs/AGENTS.md)

### Fixed

- A task created without file limits may write its unit tests (code and tests, as from chat), not just `src/`
- AIOps' pull request always has its title, summary, testing, risks and rollback; long ones are no longer cut off
- `afliz secret set` built into the app takes the first line when there is no pseudo-terminal (Windows PowerShell), as the Node CLI does since 1.1.0

## [1.1.0] - 2026-10-11

### Added

- **Monitor: commit low-risk fixes** — (Manage Agents → AIOps → Monitor, or `autoMerge` in koy.yaml)

### Fixed

- **Incidents close once they are fixed** — When the fix's change is committed and Quality's re-check passes, the incident is resolved and shows the fix commit
- The Incident Manager's triage report and post-mortem (`docs/incidents/`) go in by themselves, so they no longer hold up the fix
- **Implement can't finish without the files its goal names** — Before anything is staged, KOY checks that each file named in the goal was written (or already exists) and that something changed
- The Monitor and the Incident Manager are always on AIOps' team (and the Orchestrator's) when a project's koy.yaml turns the Monitor on, even before the…
- The agent runner's notes to the model about repeated actions no longer show up in task results, activity, chat or incident titles
- **Windows terminals** — `afliz secret set` no longer waits forever in PowerShell
- Creating a task is quicker on a busy machine: KOY looks up the base branch and commit at the same time

## [1.0.0] - 2026-10-11

### Added

- **Sub-agents** — specialists under each lead agent
- **Self-improvement (`/improve`, `afliz improve`)** — the Product Owner keeps a ranked backlog (`docs/BACKLOG.md`), and the Business Analyst proposes the top features as PRD stories with HTML mockups for you to…
- **Monitor and incidents** — AIOps: watches KOY, your running app (health URL, log file) and CI
- **Team report** — `/report`, `afliz report`, the Orchestrator's card: each agent's strengths and weaknesses, with suggested skills you add in one click
- **Accounts — your first month free** — sign in with Google, GitHub or email and every feature is unlocked for your first month: every sub-agent, any number of project folders and access tokens…
- **Welcome tour** — for newcomers: the agent team, how work flows, connecting a model, the optional account, and the choice about sharing anonymous usage (Help → Welcome tour…
- **Tasks in several project folders** — Pro: Tasks → Add folder
- **Tasks without a folder open** — every recent project's tasks, and "New task" with a project picker
- **Archive and Trash** — finished tasks are archived after 14 days and moved to the Trash 30 days later (Settings → Tasks)
- **Terminals in the app** — as many as you like at the bottom of the main area (the terminal button in the top bar)
- **Pipelines per project** — create, edit and delete them on the AIOps card, with suggestions from the project's own scripts
- **File System connector** — agents create, read, update and delete files in your home folder
- **`afliz scan`** — scans the whole project — tests, lint, typecheck and build; npm audit (and Dependabot); timings and perf scripts
- New CLI commands and SDK calls for all of the above (`afliz monitor`, `account`, `tokens`, `archive`, `trash`, `restore`, `tasks --everywhere`, `skill` …)
- **Scheduled pipelines** — Manage Agents → AIOps runs a project's pipelines by themselves — twice a month, weekly, daily or any cron schedule — while the project is open in KOY
- **Pipeline secrets** — tokens a pipeline's steps need are typed once, stored encrypted, never shown again, and given only to the steps that ask for them (scrubbed from their output)
- **Secrets only in the terminal** — API keys, tokens and client secrets are never typed into KOY
- **The Orchestrator looks after every agent** — when a task fails, it works out why and the root cause (reading the project and, with the Browser connector, the web) and tells the agent another way to try…
- **Quality reads the error logs** — errors and warnings from your app's and project's log files, failed tasks, open incidents and CI — it finds each root cause, fixes it and adds a test that…
- **Windows and Linux for Intel and ARM** — installers for x64 (Intel/AMD) and arm64 on Windows and Linux, as on macOS

### Changed

- **Closing a folder doesn't stop its agents** — Explorer lets go of it at once; tasks that are still running finish, and they keep showing in Tasks
- **Approving a task's change finishes the task** — one approval ("Approve & commit"), and the next step of a plan starts
- **Chat plans always have a button** — a plan the model writes as a list, or a one-step plan, gets "Assign to …" or "Run plan"; answering the Orchestrator's questions no longer brings more…
- **Settings → Privacy** — has one switch: share anonymous usage data (never code or prompts)
- **Manage Agents** — the agent map stays on top, with sub-agents under the agent they report to
- **Connections are in Settings** — (Settings → Connections)
- **Chat** — follows the routing profiles (no model picker) and collapses to a thin bar on the right
- **New task** — is one page, with the less common options one click away
- Line icons instead of emoji throughout

### Fixed

- Tooltips near the window's edge are no longer cut off
- A task whose description mentions "private" or "local" code no longer runs on the small local model
- After signing in or paying in the browser, KOY updates by itself (and says so); signing out and in again asks which account to use
- Manage Agents no longer crashes on an account without a plan's details; Settings → Connections' "+" works again

### Security

- Access tokens (CI, scripts) can't open a terminal or set secrets in KOY; the gateway checks its token in constant time

### Removed

- **The Beta update channel** — KOY IDE updates to stable releases only; an app on a beta moves to the next release

## [0.25.0] - 2026-10-09

### Changed

- **What's New shows only this version's changes** — after an update: only the versions since the one you had; earlier versions are one click away
- **The KOY CLI works through the open app** — with KOY IDE running, `afliz` uses it — its tasks, pipelines and AIOps watches show in the app live and use its GitHub sign-in (`--headless` runs separately)

## [0.24.0] - 2026-10-09

### Added

- **AIOps watches GitHub** — after every push the release pipeline makes, AIOps follows that commit's GitHub Actions as a task until they finish; `/aiops` shows the status, `/aiops…
- **Explore owns the PRD and TRD** — `docs/PRD.md` (product) and `docs/TRD.md` (technical) replace `docs/REQUIREMENTS.md`; every approved change updates the TRD's change log and tech stack
- **Tooltips** — icon buttons and short labels with the explanation on hover (ⓘ), so panels and Settings take less space

### Changed

- **No reinstall of the same build** — an app running a beta doesn't download the stable release made from that same commit

## [0.23.0] - 2026-10-09

### Added

- **Pipelines** — a project's own multi-step jobs in `koy.yaml`, run with `/pipeline <name>` or `afliz pipeline <name>`; every step is a task with its log, it stops at the…
- **Pause, resume and cancel local model downloads** — in Connections → Local LLM
- **Settings, redesigned** — a large window with a section list and cards that expand and collapse (Appearance, This computer, Agents & memory, Notifications, GitHub & issues, Mail,…
- **Pull requests from KOY** — `/git pr [base]` (or Source Control → PR, or the button after AIOps writes a PR) pushes the branch and opens the pull request with your GitHub sign-in —…
- **Jira** — connect through the Atlassian sign-in page; tickets (your JQL) become draft tasks, and statuses stay in step both ways — starting / finishing a task moves…
- **`/git` is the whole Git command line** — log, diff, show, blame … run at once; commit, push, pull, fetch, checkout, rename have safe flows; merge, rebase, reset, stash, add, tag … ask first
- **Manage Agents without a folder** — shows and edits the default roster and routing profiles that projects start from
- **Requirements document** — Explore keeps `docs/REQUIREMENTS.md` (business requirements, workflow logic, architecture, tech stack, technical specs, non-functional and security…
- **Quality writes test cases for the requirements** — with stub data in `test/stub-data/` that stays out of Git
- **Every agent run is a task** — the Orchestrator's planning replies and project / security scans appear in Tasks with their activity log
- **File changes in chat and Tasks** — "Change <file>" (or `/task change <path> <what>`) returns a diff in chat to apply; a task's Changes tab can add a file change, edit a staged file, and apply…

### Changed

- **Who writes which tests** — Implement writes the unit tests with the code; Quality writes the system tests (`test/system/`) and system integration tests (`test/integration/`)
- **Chat model picker** — "Auto (KOY decides for you)"
- **Push, pull and fetch without signing in to GitHub** — when KOY isn't signed in, this computer's own Git setup is used (SSH key, credential helper — osxkeychain, Git Credential Manager, `gh auth setup-git`),…
- **Configure is part of AIOps** — project configuration checks; `--agent configure` still works
- **Five agents, clear jobs** — the Orchestrator plans (no separate Plan agent); **Explore** also writes the requirements and task breakdowns; **Troubleshooter** also fixes review…
- **Feedback goes to GitHub** — Help → Send feedback… becomes an issue in koy-ide-releases — created for you when signed in to GitHub, otherwise opened prefilled in your browser
- **Start a new project** — has a cleaner screen: a large prompt box, example ideas and where the folder goes

### Fixed

- macOS: reopening KOY no longer shows What's New again after it was seen once
- Windows: starting a new project works without Git or a Git identity (it starts without Git and says so), with Git versions that don't know `init -b`, and…

## [0.22.0] - 2026-10-08

### Added

- **SDK release** — each release attaches `koy-sdk-<version>.tgz` — `npm install` it for `KoyClient` (ESM, CommonJS, TypeScript types) and `startHeadless()`
- **afliz — KOY IDE without the window** — a command line (`afliz new | chat | task | tasks | show | approve | troubleshoot | perf | status`) that runs the same Orchestrator and agents headless, or…
- **SDK** — `@koy/sdk`: a typed client for the gateway and a headless gateway you start from code
- **Start a project from Chat** — with no folder open, describe what to build — KOY creates the folder (Git + README, in ~/KOY Projects or a place you choose), opens it and hands the request…
- **The Orchestrator orders requirements** — for the best result — foundations first (setup, data model), then logic, interface, tests, docs and release; anything that uses what another creates goes…
- **Automatic retries** — a failed task runs again (up to two more attempts) as a new task that is told why the last one failed; plan steps and loops follow the retry
- **Numbered requirements** — "1. … 2. … 3. …": `/task` and chat split them into one small task per requirement, run one after another; each knows which requirement it owns and sees the…
- **Self-check** — before a code-writing agent finishes, it checks its work against each requirement (✓ with evidence / ✗ with why); an unmet requirement fails the task, so…
- **Troubleshooting closes and waives** — a fix for a GitHub issue is committed with `Fixes #n` (a merged PR closes the issue), and KOY closes the issue itself once CI passes on the fix commit (or…
- **`/troubleshoot security`** — `npm audit` and the repository's Dependabot alerts — fixable ones (a patched version, moderate or worse) go to the agents to upgrade; low-severity ones and…
- Manage Agents → **Orchestrator** introduction: what it does for you, its skills, and what to try

### Changed

- Agents answer faster: read-only agents get a smaller answer budget, every agent is told to act rather than deliberate, and detailed `/task` descriptions…
- Task worktrees link the project's `node_modules`, so an agent's `npm test` finds the project's own test tools

### Security

- Released builds — the installers, `afliz-<version>.cjs` and the SDK — contain no sandbox mode, stub model or demo data (the build fails if any is left in),…

### Fixed

- On a machine where Git has no name and email (a fresh Mac, a server, CI), approving an agent's change failed: a project started from Chat now gets its own…
- Manage Agents: the agent map no longer runs names over each other with more agents (it widens and fits each name), the Troubleshooter's icon renders, KOY's…

## [0.21.0] - 2026-10-08

### Added

- **Orchestrator** — the first agent in Manage Agents: it's the one you talk to in chat, works out what you need, and hands work to Explore, Plan, Implement, Verify and Review…
- **Run plan** — when the Orchestrator splits work into steps (e.g. Implement → Verify → Review), one button runs them as a chain — each step starts when the one before is…
- **Requirements skill (Orchestrator)** — when a request is too vague to build, the Orchestrator asks up to three short questions first, with quick-answer buttons
- **Troubleshooter** — agent and **`/troubleshoot` with no issue**: scans the open project — its test, lint, typecheck and build commands, merge-conflict markers, links that lead…
- **Performance tester** — agent and **`/perf [what]`**: times the project's tests, build and bench / perf scripts (three runs, median), reads the slow paths and reports hotspots and…
- **Agent memory and skills** — every agent (and the Orchestrator) learns while you work — your preferences from chat ("always …", "never …", "remember …"; "forget …" removes them), the…
- **Task filter** — All / Active / Review / Done / Failed / Drafts with counts, by agent, and by text; newest first; the last filter is remembered
- **Agent map** — in Manage Agents: the Orchestrator and its agents, how many tasks each got, who hands results to whom (plan and loop steps), and who is working right now
- A tip when you plan a multi-part build with only local models: add a cloud model key (one-click buttons) or split the work into steps
- `/run` confirmations offer **Always allow this exact command in this project**; `/run allowed` lists them and `/run forget <command>` asks again
- Source Control: click a changed file to see its diff against the last commit (new files show as added), with Open file
- Explorer and Source Control color changed files like VS Code — modified (M, amber), new / untracked (U / A, green), deleted (D, red), renamed (R), conflicts…

### Changed

- Routing profiles are now **Auto** (KOY decides for you), **Performance** (the best results) and **Reliable** (the lowest cost — free local models first)
- Manage Agents shows each agent's introduction only (no skills or raw instructions), written for KOY — no more "coder-1 / tester-2" from the AI Harness
- Chat replies appear smoothly: KOY waits for the first words ("… is thinking"), then the text flows in at a steady pace instead of in bursts
- Agents that write code run the project's own tests (and lint / typecheck / build when the project has them) — also outside a worktree for Verify — and a…
- Agents get the project's conventions in their brief: ESM or CommonJS, TypeScript, the test framework and where tests go, the test command and the…
- A test run that exits 0 but finds no tests (`# tests 0`, "No tests found") no longer counts as passing
- Every agent the Orchestrator assigns gets **your whole message** as its spec, so short goals no longer lose the file names, functions and behaviour you…
- On Auto, the Orchestrator plans with the Performance profile when a cloud model is usable
- Tasks use their agent's routing profile from Manage Agents unless you pick one (they used Balanced before)
- Agents get instructions written for KOY's tools instead of the AI Harness templates (which mention `gh`, ticket placeholders and other agents by name)
- Tests an agent writes must import the project's code; self-contained tests don't count
- Approving a change whose tests failed asks for confirmation first (Source Control and `/task apply` show why)
- `/troubleshoot` asks you to sign in to GitHub before it starts
- Task numbers count up without gaps (t-1, t-2 …)
- The Tasks list and the agent map load compact task items (`GET /tasks?view=compact`): 9× less data (1.3 MB → 140 KB for 500 tasks) and ~7× faster; project…
- A request that builds something and mentions tests ("… add test/x.test.ts") goes to Implement, not to the read-only Verify agent, which couldn't write the tests
- Tasks, chat sessions and change sets are saved in KOY's data folder on this device instead of `<project>/.koy/` — `git clean`, deleting `.koy/` or…
- Tasks may also write tests, `lib/`, `bin/`, `package.json` and `README.md`, not only the files they name
- Completion checks: an Implement / Fix task that changed nothing fails, Verify must end with `VERDICT: PASS` or `VERDICT: FAIL` (taken from the test run when…
- Local models: 10 minutes per call instead of 2, two retries with backoff on timeouts and busy providers, then the next suitable model

### Removed

- **Slack** — sign-in, notifications, `koy …` channel commands and the Slack app connection
- `/editfile` — describe the change with `/task …` instead; the agent makes it and you review it
- The AFLIZ loops and `/afliz` (pm, build, review, aiops, configure)

### Fixed

- **Security** — a symlink inside a project that pointed outside it (e.g. `keys -> ~/.ssh`) let the Explorer, chat and agents read or write there; every path check now…
- A stray rejected promise can no longer take the gateway down, and unexpected errors are logged with secrets scrubbed
- macOS updates on unsigned builds: "Restart now" failed with "Could not get code signature for running application"
- **New chat** — (+) switches to the new conversation right away (it used to stay on the current one until a reload)
- Chat formatting: list items with `code` or **bold** no longer break into columns; `_italic_` and `> quote` blocks render
- A finished Explore / Plan / Verify / Review task no longer says it "changed no files — describe the task again"
- Tasks created at the same time could start before their worktree existed, so their agent couldn't run commands
- Pressing ⌘S / Ctrl+S right after typing could skip the save, and the first keys typed into a rename / new-file box that had just opened could be lost

### Security

- **Electron 44** — from 33: fixes the published Electron advisories — sandbox and context-isolation bypasses, cross-origin reads through custom protocols, use-after-free…
- **electron-builder 26** — the bundled `electron-updater` runtime no longer leaks credentials on cross-origin redirects, Linux AppImages no longer search untrusted library paths, and…

## [0.20.0] - 2026-10-08

### Added

- **Slack** — Sign in with Slack (OAuth screen in the browser), notifications to your channel, and commands — post `koy <command>` in the channel (`koy run the tests`,…
- **Feedback → Google Sheet** — Help → Send feedback… (or `/feedback`) sends the text with an anonymous client id, timestamp, version and OS to one feedback sheet for every install (via a…
- **CI → GitHub issues** — failed jobs and warnings in CI, Release and Nightly runs open (or update) GitHub issues, and close them when the job passes again
- **Troubleshooting agents** — `/troubleshoot <issue>` collects the issue and its CI logs, then Explore → Plan → Implement → Verify fix it (with your approval) and comment the result on…

### Security

- Secrets are never shown: the gateway token no longer appears on the command line, DevTools are off in installed builds, `/run` refuses to print credential…

### Changed

- GitHub Actions use `actions/checkout@v5` and `actions/setup-node@v5` (Node 24); builds use Node 22

## [0.19.0] - 2026-10-08

### Added

- AI Harness agents, **one per job**: Explore, Plan, Implement, Verify and Review form the roster; Fix, Create PR, Requirements, Task breakdown, Configure and…
- **AFLIZ loops in KOY** — the harness's `afliz` CLI as chat commands: `/afliz build` (explore → plan → implement → verify, with fix rounds), `/afliz pm` (requirements → task list →…
- `npm run harness:sync` pulls the latest https-afliz-com/ai-harness from GitHub (a clone next to this repository) before regenerating the agent library;…
- License: the Business Source License 1.1 from Anh Luu Services Co
- The public koy-ide-releases repository has its own CHANGELOG.md (released versions, links to their release pages), refreshed automatically after every…

### Changed

- Chat `/run` runs in the open project instead of requiring a task: safe commands (tests, lint, build, typecheck, git status/diff/log, the project's own…

## [0.18.1] - 2026-10-08

### Added

- Update channels: **Stable** (tagged releases) and **Beta** (nightly builds of the latest code), in Settings → About & updates
- Nightly builds: every night, if the code changed, GitHub Actions builds macOS, Windows and Linux and publishes a beta pre-release; old nightlies are pruned
- Releases and the update feed are published to the public `https-afliz-com/koy-ide-releases` repository, so installed apps can update while the source stays…

## [0.18.0] - 2026-10-08

### Added

- Code signing and notarization setup: hardened runtime and entitlements for macOS, Windows Authenticode through environment variables, `npm run…
- Release automation: pushing a version tag builds, signs and uploads macOS (Apple silicon and Intel), Windows and Linux installers plus the update feed to…

### Fixed

- Chat now knows the open project: every message carries its files, stack (package.json scripts and dependencies, other manifests), Git branch, changes and…
- The chat model can read, list and search project files and check Git status on its own before answering (shown as “Looked at: …”); secret files and…
- “Run the tests / build / lint” runs this project's own command (e.g. `npm test`) instead of the literal words
- Local models no longer invent file contents after asking to read a file, and get a context window large enough for the project overview

## [0.17.0] - 2026-10-07

### Added

- Versioned releases with this changelog: `npm run bump` sets the version in every package and dates the release notes
- Software updates: KOY IDE checks for a new version at launch and every 4 hours, checks free space before downloading, notifies you when the update is ready,…
- **Help → Check for Updates…** — and **Help → What's New**; Settings → About shows the version, update status and where updates are downloaded on this computer

## [0.16.0] - 2026-10-07

### Added

- Cross-platform installers: macOS (dmg, zip), Windows (NSIS), Linux (AppImage, deb)
- First-launch device check that recommends a local model size, parallel local runners and a routing profile
- DeepSeek provider; parallel agent runners for cloud and local models
- Chat sessions in Auto routing, `/` commands and `@` file mentions; mail through the OS mail app
- GitHub sign-in in the default browser, organization repositories, clone and connect existing repositories

[2.5.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v2.5.0
[2.4.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v2.4.0
[2.3.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v2.3.0
[2.2.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v2.2.0
[2.1.1]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v2.1.1
[2.1.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v2.1.0
[2.0.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v2.0.0
[1.1.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v1.1.0
[1.0.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v1.0.0
[0.25.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.25.0
[0.24.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.24.0
[0.23.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.23.0
[0.22.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.22.0
[0.21.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.21.0
[0.20.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.20.0
[0.19.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.19.0
[0.18.1]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.18.1
[0.18.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.18.0
[0.17.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.17.0
[0.16.0]: https://github.com/https-afliz-com/koy-ide-releases/releases/tag/v0.16.0
