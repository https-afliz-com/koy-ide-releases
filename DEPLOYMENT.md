# Deploying KOY IDE headless (afliz)

KOY IDE's gateway — the Orchestrator, the agents, Workspace Trust, the tool gateway, approvals, agent memory — also runs
**without the desktop window**. Use it on a build server, in CI, on a team machine, or behind your own tools through the
SDK. The same gateway serves the desktop app, so everything behaves the same.

## What you need

- **Node.js 20 or newer** (22 recommended) and **Git**.
- `afliz-<version>.cjs` from the [releases page](https://github.com/https-afliz-com/koy-ide-releases/releases) — one
  self-contained file, nothing to install. It is a **production build**: no sandbox mode, no stub model, no demo data —
  every answer comes from your models. (For the SDK: `koy-sdk-<version>.tgz` on the same page — see SDK.md.)
- A model: an API key for a cloud provider, or Ollama / LM Studio on the machine (or reachable from it).

## 1. Run it once from the command line

```bash
node afliz-0.21.0.cjs --version
cd /path/to/project
node afliz-0.21.0.cjs status --trust
```

Without `--url`, every command starts its own gateway in-process and stops it when it's done. State — tasks, chat,
change sets, agent memory, encrypted keys — lives in the **data folder** (`~/.koy/headless`, or `--data <dir>` /
`KOY_DATA_DIR`). Keep it on persistent, private storage; back it up like any other secret store.

`--trust` grants Workspace Trust to a folder the first time (agents only ever work in trusted folders). Use it only for
folders you'd let an agent change.

## 2. Run it as a service (`afliz serve`)

```bash
KOY_TOKEN="$(openssl rand -hex 24)" node afliz-0.21.0.cjs serve --port 4317 --host 127.0.0.1 --data /var/lib/koy
```

- **Token:** every request needs `Authorization: Bearer <token>`. Set `KOY_TOKEN` yourself (a long random value, kept in
  your secret manager); otherwise a new one is printed at start (`--quiet` hides it).
- **Network:** it listens on `127.0.0.1` by default. To reach it from other machines, put it behind a reverse proxy with
  TLS (nginx, Caddy) and keep the port closed to the internet — the token is the only lock. `--host 0.0.0.0` only on a
  trusted network.
- **API docs:** `http://<host>:<port>/api/v1/docs` (OpenAPI: `/api/v1/openapi.json`).

### systemd (Linux)

```ini
# /etc/systemd/system/koy.service
[Unit]
Description=KOY IDE gateway (headless)
After=network-online.target

[Service]
User=koy
WorkingDirectory=/srv/projects
EnvironmentFile=/etc/koy/env          # KOY_TOKEN=…  (chmod 600)
ExecStart=/usr/bin/node /opt/koy/afliz.cjs serve --port 4317 --data /var/lib/koy
Restart=on-failure
NoNewPrivileges=true

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload && sudo systemctl enable --now koy
```

### Docker

```dockerfile
FROM node:22-slim
RUN apt-get update && apt-get install -y --no-install-recommends git ca-certificates && rm -rf /var/lib/apt/lists/*
COPY afliz-0.21.0.cjs /opt/koy/afliz.cjs
USER node
VOLUME ["/data", "/projects"]
EXPOSE 4317
CMD ["node", "/opt/koy/afliz.cjs", "serve", "--host", "0.0.0.0", "--port", "4317", "--data", "/data"]
```

```bash
docker run -d -p 127.0.0.1:4317:4317 -e KOY_TOKEN -v koy-data:/data -v "$PWD:/projects" koy-headless
```

## 3. Models and keys

Add providers once — from a desktop app pointed at the server, or through the API (`POST /api/v1/providers`, then
`PATCH /api/v1/providers/{id}` with the key). Keys are stored encrypted in the data folder and never returned. For a
local model, run Ollama next to the gateway and set the provider endpoint (`PATCH /api/v1/providers/ollama`).

## 4. CI

```yaml
- run: curl -sSLo afliz.cjs https://github.com/https-afliz-com/koy-ide-releases/releases/download/v0.21.0/afliz-0.21.0.cjs
- run: node afliz.cjs troubleshoot --wait --trust --data "$RUNNER_TEMP/koy"      # scan: tests, lint, typecheck, build, risks
- run: node afliz.cjs perf --wait --trust --data "$RUNNER_TEMP/koy"              # Quality's performance report
```

Agents never push or merge: changes are committed in the job's checkout only when you pass `--approve`, so review them
(the output shows each diff) before you push them anywhere.

## 5. Updating

Replace `afliz.cjs` with the new version's file and restart the service. The data folder stays compatible between
versions; take a copy before a major upgrade.

## Security checklist

- A long random `KOY_TOKEN`, stored as a secret; the port bound to localhost or behind TLS.
- The data folder readable only by the service user (`chmod 700`).
- `--trust` only on folders the agents may change; run the service as a user that can't write elsewhere.
- Review approvals: `--approve` applies changes without asking — use it in CI only on throwaway checkouts.
