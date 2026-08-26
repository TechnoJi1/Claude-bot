# Mineflayer Worker Bot — Standalone (Railway-ready)

Plain npm project. No pnpm, no workspace, no leftover Replit scaffolding.
This entire folder should be the **root** of your GitHub repo.

## 1. Configure

**Option A — local file (simplest for local testing):**
```bash
cp settings.example.json settings.json
```
Edit `settings.json` with your server host/port, bot username, `access.commandUsernames`,
`home.coordinates`, `home.chest`, and module toggles.

**Option B — environment variable (recommended for Railway):**
Since `settings.json` is gitignored (it can contain secrets), on Railway set an
environment variable instead:

- Key: `MINECRAFT_SETTINGS_JSON`
- Value: the **entire contents** of your filled-in `settings.json`, minified to one line

The bot automatically falls back to this env var if `settings.json` isn't found on disk
(this was added in `src/config.ts` — no code changes needed from you).

## 2. Test locally

```bash
npm install
npm start
```
Look for `[Bot] spawned and ready` in the console. Try `!status` in-game from a
username listed in `access.commandUsernames`.

## 3. Deploy to Railway

1. Push this folder's contents as the **root** of your GitHub repo (flat structure —
   `package.json` at the top level, not nested in a subfolder).
2. Railway → New Project → Deploy from GitHub repo.
3. This repo includes `railway.json` already configured for plain `npm install` / `npm start` —
   no pnpm anywhere. If Railway still tries to run pnpm, it means an old `railway.json`
   or `pnpm-lock.yaml` is still committed somewhere in your repo history — delete it.
4. Add `MINECRAFT_SETTINGS_JSON` as an environment variable (see Option B above) so you
   don't need to commit `settings.json` at all.

## Why the previous deploy failed

Your repo's root `railway.json` was explicitly telling Railway to run
`pnpm install --frozen-lockfile` and `pnpm --filter @workspace/mineflayer-bot start` —
leftover from Replit's monorepo scaffold. That's now replaced with plain npm commands.
Make sure your GitHub repo's root **only** contains the files in this package —
delete any old `pnpm-workspace.yaml`, old root `package.json`, `tsconfig.base.json`,
`artifacts/`, `lib/`, `scripts/`, `.replit`, `attached_assets/` etc. if they're still there.

## Chat commands (from a whitelisted username)

| Command | Effect |
|---|---|
| `!mine <block_name>` or `!mine` | Queue a mining task |
| `!farm` | Queue a farming pass |
| `!guard` | Stay near home, auto-fight hostiles in range |
| `!stop` | Clear queue, stop current task, return to idle |
| `!status` | Bot replies with state, HP, inventory fullness |
