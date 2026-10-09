# afliz — KOY IDE on the command line

`afliz` runs KOY IDE without its window: the same Orchestrator and agents, Workspace Trust, approvals and checks — from a
terminal, a script or CI. By default it starts its own gateway for each command; with `--url` it uses a running one
(the desktop app's, `afliz serve`, or a server).

## Install

Each release has `afliz-<version>.cjs` on the [releases page](https://github.com/https-afliz-com/koy-ide-releases/releases)
— one file, nothing to install, Node.js 20 or newer.

```bash
curl -sSLo afliz.cjs https://github.com/https-afliz-com/koy-ide-releases/releases/download/v0.22.0/afliz-0.22.0.cjs
node afliz.cjs --version
alias afliz="node $PWD/afliz.cjs"      # optional
```

Add a model first: an API key for a cloud provider, or a local model through Ollama (see *Models* below).

## Commands

| Command | |
|---|---|
| `afliz new "<what to build>" [--dir <parent>]` | Start a new project (folder + Git + README) and send the request to the Orchestrator |
| `afliz chat "<message>" [--do "<action>"]` | Talk to the Orchestrator in this project; `--do "Run plan"` runs one of its actions |
| `afliz task "<goal>" [--agent <key>]` | One agent task; numbered requirements (1. 2. 3.) become a plan, in the best order |
| `afliz tasks [--status s] [--agent a] [--q text]` | List tasks (status: `active`, `review`, `done`, `failed`, `drafted`) |
| `afliz show <id>` · `afliz approve <id>` | A task's result and evidence · apply & commit its change |
| `afliz troubleshoot [<issue> \| security]` | Scan this project / fix a GitHub issue / handle vulnerabilities |
| `afliz perf [what]` | Quality measures this project's performance |
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

afliz is part of KOY IDE and licensed under the Business Source License 1.1.
