# afliz — KOY IDE 2 on the command line

`afliz` runs KOY IDE 2 without its window: the same Orchestrator and agents, Workspace Trust, approvals and checks — from a
terminal, a script or CI. By default it starts its own gateway for each command; with `--url` it uses a running one
(the desktop app's, `afliz serve`, or a server).

## Install

Each release has `afliz-<version>.cjs` on the [releases page](https://github.com/https-afliz-com/koy-ide-releases/releases)
— one file, Node.js 20 or newer — and `koy-gateway-<version>-<os>-<arch>`, the gateway afliz starts when it runs on
its own. Put both in one folder (or `koy-gateway` on your `PATH`, or set `KOY_GATEWAY_BIN`).

```bash
curl -sSLo afliz.cjs https://github.com/https-afliz-com/koy-ide-releases/releases/download/v1.1.0/afliz-1.1.0.cjs
curl -sSLo koy-gateway https://github.com/https-afliz-com/koy-ide-releases/releases/download/v1.1.0/koy-gateway-1.1.0-macos-arm64
chmod +x koy-gateway
node afliz.cjs --version
alias afliz="node $PWD/afliz.cjs"      # optional
```

Inside KOY IDE 2's terminals `afliz` is already there — no Node.js needed for the commands the app asks you to run
(`afliz secret set …`) and the everyday ones (`status`, `chat`, `task`, `tasks`, `show`, `approve`, `scan`,
`pipelines`, `pipeline`).

Add a model first: an API key for a cloud provider, or a local model through Ollama (see *Models* below).

## Commands

| Command | |
|---|---|
| `afliz new "<what to build>" [--dir <parent>]` | Start a new project (folder + Git + README) and send the request to the Orchestrator |
| `afliz chat "<message>" [--do "<action>"]` | Talk to the Orchestrator in this project; `--do "Run plan"` runs one of its actions |
| `afliz task "<goal>" [--agent <key>]` | One agent task; numbered requirements (1. 2. 3.) become a plan, in the best order |
| `afliz tasks [--status s] [--agent a] [--q text]` | List tasks (status: `active`, `review`, `done`, `failed`, `drafted`, `archived`, `trash`) |
| `afliz tasks --everywhere` | Every recent project's tasks — no project needed |
| `afliz archive <id>` · `afliz trash <id>` · `afliz restore <id>` · `afliz empty-trash` | Archive a finished task · move it to the Trash · bring it back · delete the Trash for good |
| `afliz show <id>` · `afliz approve <id>` | A task's result and evidence · apply & commit its change |
| `afliz troubleshoot [<issue> \| security]` | Scan this project / fix a GitHub issue / handle vulnerabilities |
| `afliz perf [what]` | Quality measures this project's performance |
| `afliz improve [--auto-merge off] [--wait]` | KOY's product team: backlog, proposals with mockups, low-risk fixes (KOY Pro) |
| `afliz scan [--no-security] [--no-perf]` | Scan the whole project — tests, lint, typecheck, build; npm audit (+ Dependabot); timings and perf scripts. Exit 1 on problems |
| `afliz report` · `afliz skill <agent> "<text>"` | The team report (strengths, weaknesses, suggested skills) · give an agent a skill |
| `afliz monitor [on\|off\|check\|fix <id>\|dismiss <id>] [--health-url u] [--log-file f]` | AIOps › Monitor and its incidents (KOY Pro) |
| `afliz account [sign-in\|sign-out\|refresh\|plans]` | Your account and plan |
| `afliz tokens [create <name>\|revoke <id>]` | Access tokens (KOY Pro): named bearer tokens for the CLI, the SDK and CI |
| `afliz pipelines` | The project's pipelines (koy.yaml) |
| `afliz pipeline <name> [--only s] [--from s] [--yes]` | Run one: each step a task, stops at the first failure; `--yes` approves its commands |
| `afliz smoke` | The project's `smoke` pipeline |
| `afliz secret <NAME>` | Set a pipeline secret (e.g. `RELEASES_TOKEN`): typed without echo or piped in, stored encrypted, never printed |
| `afliz status` | Gateway, project, tasks, agent memory |
| `afliz serve [--port 4317] [--host 127.0.0.1]` | A headless gateway for the SDK, CI or a remote app (see the deployment notes) |

Agents: `explore`, `implement`, `troubleshoot`, `quality`, `aiops` (and any you added to the roster). Older names still
work: `plan`, `requirements`, `tasks` → Explore; `verify`, `review`, `perf` → Quality; `fix` → Troubleshooter; `pr`, `configure` → AIOps.

## Options

| | |
|---|---|
| `--project <dir>` | The project (default: the current folder) |
| `--trust` | Grant Workspace Trust to a folder KOY hasn't seen — agents only work in trusted folders |
| `--wait` | Wait until the work (and the plan steps after it, and retries) is finished |
| `--approve` | With `--wait`: apply & commit each change as it's ready. Without it, changes wait for `afliz approve <id>` |
| `--json` | Machine-readable output |
| `--url <url>` `--token <t>` | Use a running gateway (or `KOY_URL` / `KOY_TOKEN`) |
| `--data <dir>` | Where the gateway keeps its state and encrypted keys (default `~/.koy/headless`, or `KOY_DATA_DIR`) |

Exit codes: `0` done, `1` something failed (a task, a check, a troubleshoot run), `2` wrong usage.

## Examples

```bash
afliz new "A counter library: 1. a Counter class in src/counter.js 2. tests" --trust --do "Run plan" --wait
afliz chat "What does this project do?" --trust
afliz task "add input validation to src/api.ts" --agent implement --wait      # then: afliz approve t-3
afliz troubleshoot --wait          # tests, lint, typecheck, build, risks — and fixes for what fails
afliz troubleshoot security --wait --approve
afliz perf --wait --json > perf.json
```

## Models

```bash
afliz serve &          # then, with its token:
curl -X PATCH http://127.0.0.1:4317/api/v1/providers/anthropic -H "Authorization: Bearer $KOY_TOKEN" \
     -H 'content-type: application/json' -d '{"apiKey":"…"}'
```

Or point it at Ollama (`PATCH /api/v1/providers/ollama` with `{"endpoint":"http://127.0.0.1:11434"}`). Keys are stored
encrypted in the data folder and never shown again.

## Security

- Agents never push or merge, and work only in folders you trusted (`--trust`).
- `--approve` applies changes without asking you — use it only on throwaway checkouts (CI); otherwise review with
  `afliz show` and approve with `afliz approve`.
- `afliz serve`: set a long random `KOY_TOKEN`, bind to localhost or put it behind TLS.

## Production build vs development build

The release file is a **production build**: it has no sandbox mode, no stub model and no demo data — every answer
comes from your models. (`--sandbox` exists only in the development build inside the source repository, for tests.)

afliz is part of KOY IDE 2 and licensed under the Business Source License 1.1.

## Secrets: `afliz secret set <slot>`

KOY never asks for a secret in its screens or in chat. API keys, tokens and client secrets are entered in a terminal —
`afliz secret set <slot>` asks for the value without showing it (or reads it from a pipe) — and kept in the operating
system's keychain (macOS Keychain, Windows Credential Manager, Linux Secret Service), not in KOY's files. In the app, each
"Set in terminal" button opens KOY's terminal with the command running (`afliz` is on the PATH there).

| Slot | What |
|---|---|
| `provider:<id>` | A model provider's API key — e.g. `provider:deepseek`; KOY tests the connection right away |
| `github:token` | A GitHub personal access token (signs in) |
| `github:oauth-secret` | Your GitHub OAuth App's client secret |
| `jira:client-secret` | Your Atlassian OAuth app's client secret |
| `app:<app>:<field>` | A connected app's secret field (then Connect in KOY) |
| `pipeline:<project>:<name>` | A secret a pipeline's steps get as an environment variable |

`afliz secret delete <slot>` removes one. In chat, `/secret <slot>` opens the terminal the same way.
