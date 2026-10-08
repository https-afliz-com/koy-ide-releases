# KOY IDE SDK (`@koy/sdk`)

Drive KOY IDE from code: open a project, talk to the **Orchestrator**, run agent tasks and plans, review and approve
their changes, troubleshoot, measure performance. The SDK talks to a KOY gateway — the desktop app's, one started with
`afliz serve`, a server you deployed, or one it starts in your own process (`startHeadless`). Every call goes through
the same gateway the app uses, so Workspace Trust, the tool gateway, approvals and the agents' checks all apply.

## Install

Each release has `koy-sdk-<version>.tgz` on the [releases page](https://github.com/https-afliz-com/koy-ide-releases/releases):

```bash
npm install https://github.com/https-afliz-com/koy-ide-releases/releases/download/v0.22.0/koy-sdk-0.22.0.tgz
```

Node.js 20+ (the client also runs in Deno, Bun and browsers — anything with `fetch`). ESM and CommonJS, with TypeScript
types. The client and the gateway must have the **same version**.

## Connect

```ts
import { KoyClient } from '@koy/sdk';

// A running gateway: the desktop app's, `afliz serve`, or your server (token: KOY_TOKEN or `afliz serve` output).
const koy = new KoyClient({ url: 'http://127.0.0.1:4317', token: process.env.KOY_TOKEN! });
```

or start one in this process (production — your models and keys, stored encrypted in `dataDir`):

```ts
import { startHeadless } from '@koy/sdk/headless';
const { client: koy, close } = await startHeadless({ dataDir: '/var/lib/koy' });
// …
await close();
```

## Work with the Orchestrator

```ts
await koy.openProject('/path/to/project', { trust: true });       // trust: grant Workspace Trust the first time
const r = await koy.chat('Add a multiply(a, b) function to src/math.js with tests');
console.log(r.reply);                                              // the Orchestrator's answer (Markdown)
console.log(r.actions.map((a) => a.label));                        // e.g. "Run plan (5 steps: Explore → …)"
const ids = await koy.runAction(r.actions, 'Run plan');           // like clicking the button
const tasks = await koy.waitForTasks(ids, {
  approve: true,                                                   // apply & commit changes as they are ready
  onStatus: (t) => console.log(t.id, t.role, t.status),
});
```

`approve: true` applies every change the agents make, without asking. Leave it off to review them yourself:
`waitForTasks` then returns when a change is waiting, and `koy.approve(id)` applies it.

## API

| Call | What it does |
|---|---|
| `health()` | Gateway version and environment |
| `openProject(path, { trust })` · `createProject(description, { parentDir, trust })` | Open a folder · start a new project from a description |
| `chat(text, sessionId?)` | A message (or `/command`) to the Orchestrator → `{ reply, actions, taskIds, session }` |
| `runAction(actions, label)` | Run one of a reply's actions ("Run plan", "Assign to Implement …") → task ids |
| `tasks({ status, agent, q })` · `task(id)` | List (status: `active`, `review`, `done`, `failed`, `drafted`) · one task |
| `waitForTasks(ids, { approve, approveFailing, onStatus, timeoutMs })` | Wait for tasks, the plan steps after them and their retries |
| `approve(id, { approveFailing })` · `cancel(id)` | Apply & commit a task's change (refused when its tests failed, unless `approveFailing`) · cancel |
| `troubleshoot()` · `troubleshoot(42)` · `troubleshoot('security')` | Scan the project · fix GitHub issue #42 · handle vulnerabilities → a loop run |
| `waitForLoop(id, { approve })` | Wait for a troubleshoot run |
| `memory()` | How much the agents have learned (counts only) |
| `settings(patch?)` | Read or change settings |
| `request(method, path, body?)` | Any other API call (`/api/v1/docs` on the gateway lists them) |

Errors are `KoyError` with the gateway's `code` (`TRUST_REQUIRED`, `TESTS_NOT_PASSING`, `TIMEOUT` …) and `status`.

## Security

- Treat the gateway token like a password; bind `afliz serve` to localhost or put it behind TLS.
- `trust: true` lets agents change that folder; `approve: true` applies their changes without review — use both only
  where that's what you want (e.g. a throwaway CI checkout).
- Keys you add are stored encrypted in the gateway's data folder and are never returned by the API.

The SDK is part of KOY IDE and licensed under the Business Source License 1.1 (see LICENSE).
