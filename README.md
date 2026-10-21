![Deploy log](banner.svg)

# Deploy log

Every deploy, every git pull and every published site with time, state (commit and snapshot) and result – per project, with one click back to an earlier state.

[Deutsch](README.de.md) · An official [Hostwerk](https://github.com/Passimoritz/hostwerk-public) extension (SDK 2.7, native extension)

## What it does

- **Where:** Projects → Deploy log (all projects) and the tab of every project – no own sidebar entry since 1.1.0; the full page stays under Extensions → Open
- Logs on its own, nothing to set up: **Publish** to a server (SFTP, FTP, git push), **git pull** in Hostwerk and **Publish sites** (GitHub Pages, Cloudflare Pages, extension [static-publish](https://github.com/Passimoritz/hostwerk-ext-static-publish)); restoring a snapshot shows up as well
- Every entry: time and duration, target, result (files uploaded/removed, address, or the error), the **commit** (branch, short hash, message, and how many files differed from it) and the **snapshot** of that state
- **Back to the previous state** with one click – or back to any entry: Hostwerk saves the current state as a snapshot first, then restores the project files of that time; if you want, the old state is published again to the same server or site right away
- Own page with all projects, a **“Deploy log” tab on every project** and a dashboard tile with the newest entries; right-click an entry to go back, copy the commit hash or open the address
- Settings: take a snapshot after every successful deploy, pull or publish (default on; without it the newest existing snapshot is noted), how many of its own snapshots to keep per project (default 10 – snapshots you made yourself are never touched)

## How it works

The extension only uses what Hostwerk already offers: it listens to Hostwerk's own events (deploy and publish jobs, the change log for git pull, snapshot restores), reads the commit with Hostwerk's Git service and uses Hostwerk's snapshots and deploy service for going back. Databases are never rolled back – only project files.

Limits: a **failed** git pull is not logged (Hostwerk only reports successful ones), and a target of type “git push” is not pushed again when going back – restoring files does not change commits.

## Install

In Hostwerk: **Extensions → Discover** and click “Install”.

Native extensions run as part of Hostwerk (own server and interface part). That is why Hostwerk only installs them from the official catalogue (with checksum) or in developer mode.

## Data

`data/deploy-log/log.json` – up to 200 entries per project (no passwords or target settings, only name and type of the target). Snapshots live where Hostwerk keeps all snapshots (`backups/snapshots/<project>`). Removing the extension keeps both.

## Develop

```bash
node --test tests/*.test.js          # tests with a simulated Hostwerk (events, snapshots, deploy)
node build-native.js .               # build the interface (sdk/build-native.js from hostwerk-public)
node check.js . --strict             # same check as the catalogue (sdk/check.js)
```

Event recognition and bookkeeping in `server/log.js` (no Hostwerk needed), wiring in `server/index.js`, interface in `ui/src`. Texts in the code are English, German is in `locales/de.json`.

## License

MIT
