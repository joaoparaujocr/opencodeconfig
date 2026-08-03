# OpenCode config

Shared OpenCode setup (`~/.config/opencode`).

## Setup

```bash
git clone git@github.com:joaoparaujocr/opencodeconfig.git ~/.config/opencode
cd ~/.config/opencode
npm install
```

If `~/.config/opencode` already exists, back it up first:

```bash
mv ~/.config/opencode ~/.config/opencode.bak
git clone git@github.com:joaoparaujocr/opencodeconfig.git ~/.config/opencode
cd ~/.config/opencode && npm install
# merge anything unique from ~/.config/opencode.bak if needed
```

## Environment

Providers use env vars (no secrets in git). Set at least:

- `DEVWORLD_API_KEY`
- `DEVWORLD_API_BASE_URL`

## Optional local plugin: opencode-dw

`plugins/opencode-dw` is **not** in this repo (machine-local symlink).

On a machine that has the project:

```bash
ln -s /path/to/opencode-dw ~/.config/opencode/plugins/opencode-dw
```

## Optional: rtk

`plugins/rtk.ts` needs [`rtk`](https://github.com) `>= 0.23.0` on `PATH`. Without it the plugin disables itself.

## Layout

| Path | Purpose |
|------|---------|
| `opencode.jsonc` | Main config (models, agents, permissions) |
| `opencode.json` | Extra plugins |
| `agents/` | Custom agents |
| `commands/` | Slash commands |
| `skills/` | Skills |
| `plugin/` | Local plugin assets (e.g. shell-strategy) |
| `plugins/` | Plugin entrypoints (e.g. `rtk.ts`) |
| `prompts/` | Shared prompts |
| `package.json` | Plugin deps (`npm install`) |
