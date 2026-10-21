# Relay for Home Assistant

A Home Assistant add-on that puts Claude (via your Claude Pro plan) and ChatGPT (via your ChatGPT Plus plan) in one chat panel, with automatic routing between a cheap chat mode and a full agent mode.

## Install

**As a local add-on (quickest)**

1. Copy the `relay` folder into `/addons/` on your Home Assistant host (Samba or SSH add-on).
2. Go to **Settings > Add-ons > Add-on Store**, open the menu, select **Check for updates**.
3. **Relay** appears under *Local add-ons*. Install it (the first build takes a few minutes).

**From GitHub**

1. Push this folder to a GitHub repository and set the URL in `repository.yaml` and `relay/config.yaml`.
2. In the Add-on Store menu, select **Repositories** and add the repository URL.

Then follow `relay/DOCS.md` (also shown on the add-on's Documentation tab).

## Layout

```
relay/
  config.yaml          add-on definition (ingress panel, options)
  Dockerfile
  DOCS.md
  translations/en.yaml option labels
  app/
    server.js          HTTP + streaming API, served through ingress
    lib/router.js      Auto routing rules (no model call)
    lib/claude.js      Claude Agent SDK runner
    lib/codex.js       Codex SDK runner
    lib/auth.js        ChatGPT device login
    lib/ha.js          read-only Home Assistant lookups (Supervisor API)
    mcp/ha-mcp.js      MCP server exposing those lookups to both agents
    lib/store.js       chat history as JSON files in /data
    public/            the panel UI
```

## Local development

```
cd relay/app && npm install
RELAY_DEV=1 RELAY_DATA=./.dev/data RELAY_SHARE=./.dev/share HOME=./.dev/home CODEX_HOME=./.dev/home/.codex node server.js
```

`RELAY_DEV=1` turns off the ingress-only check so you can open `http://localhost:8099`. To test the Home Assistant tools outside an add-on, also set `RELAY_HA_URL=http://<your-ha>:8123/api` and `RELAY_HA_TOKEN=<a long-lived access token>`.

SDK versions are pinned in `app/package.json`. Bump them deliberately, and prefer releases that have been out for a couple of weeks.
