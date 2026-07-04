<div align="center">

# XA Dashboard

**A fast, local web dashboard for your FINAL FANTASY XIV retainers, submarines, free company, and marketboard — in a single executable. No Python, no install, no telemetry.**

[![Download](https://img.shields.io/badge/download-aethertek.io-8b5cf6)](https://aethertek.io/applications/xa-dashboard.html)
[![Windows](https://img.shields.io/badge/Windows-x64-0078D6)](https://aethertek.io/applications/xa-dashboard.html)
[![Linux](https://img.shields.io/badge/Linux-x64-FCC624)](https://aethertek.io/applications/xa-dashboard.html)
[![License](https://img.shields.io/badge/license-Proprietary-red)](LICENSE)

</div>

---

## What is it?

XA Dashboard reads the data your FFXIV plugins already save on disk and presents it as a clean, fast, **local** web dashboard in your browser. Run one program, open the page, and see every account, character, retainer, and submarine across your whole setup at a glance.

It is **read-only**: it never writes to your game, plugin configs, or plugin databases. Everything stays on your machine — there is no account, no login, and nothing is sent anywhere.

### Highlights

- **Dashboard** — every account and character with gil, FC points, housing, treasure, ready/idle retainers and submarines, income/cost, and restock totals.
- **FC Data** — housing plot overview, FC capacity planner, and submarine planners.
- **Data** — a sortable master submarine table with build/restock/inventory status and Excel export.
- **Market** — your retainers' marketboard listings, grouped by account or as a single "all items" undercut view, with a privacy toggle.
- **Charts** — financial history (wealth, earnings, profit, fleet, supply, consumption) with hover crosshairs.
- **Mass item search** across inventories, saddlebag, armoury, equipped gear, crystals, retainers, and market listings.
- **Market Monitor** *(Windows)* — optionally watches your own listings and posts batched "listing changed" alerts to a Discord webhook.
- **Privacy toggle** to blur account / character / retainer / world names for streaming or screenshots.

> Works alongside AutoRetainer, Lifestream, and XA Database data.

---

## Download

Grab the latest build from the official page — **https://aethertek.io/applications/xa-dashboard.html**:

| Platform | File | Notes |
|---|---|---|
| **Windows 10/11 (x64)** | `XA-Dashboard-Setup-vX.Y.Z.exe` | **Recommended.** Per-user installer (no admin), Start Menu entry, uninstaller |
| **Windows 10/11 (x64)** | `XA-Dashboard-vX.Y.Z-windows.zip` | Portable — unzip anywhere and run |
| **Linux (x64)** | `XA-Dashboard-vX.Y.Z-linux.zip` | Runs headless; open the printed URL yourself |

---

## Quick start

### Windows (installer)

1. Download and run `XA-Dashboard-Setup-vX.Y.Z.exe`.
2. Accept the license agreement — the app installs per-user (no admin rights needed).
3. Launch **XA Dashboard** from the Start Menu; a small status window opens and your browser launches at **http://127.0.0.1:21005/**.

### Windows (portable)

1. Download and unzip `XA-Dashboard-vX.Y.Z-windows.zip` anywhere (e.g. `Documents\XA Dashboard`).
2. Double-click **`xa-dashboard.exe`**.

> Windows may show a SmartScreen prompt for a new unsigned app — choose **More info → Run anyway**.

### Linux

1. Download and unzip `XA-Dashboard-vX.Y.Z-linux.zip`.
2. In a terminal, from the unzipped folder:
   ```bash
   ./run.sh          # or:  ./xa-dashboard
   ```
3. Open **http://127.0.0.1:21005/** in your browser.

> The Linux build runs as a headless server (no native window). It is ideal for a home server / second machine.

---

## First-run configuration

On first launch the app creates a config in your user profile and seeds it from `config.json.example`, so it runs immediately.

**Single account, default install? No editing needed.** The seeded `Main` account already points at XIVLauncher's default plugin folder — `%APPDATA%\XIVLauncher\pluginConfigs` on Windows, `~/.xlcore/pluginConfigs` on Linux — so your data appears on the very first run. (`%VAR%` and `~` in paths are expanded automatically.)

For more accounts, enable and edit the bundled `Acc2` / `Acc3` template rows: open **Settings** (button in the window, or http://127.0.0.1:21005/settings/) and either:

- **Import settings** — import an existing Python AutoRetainer dashboard `config.json` (accounts, item values, plans, rates, highlights, display options); or
- Add each account's `pluginConfigs` path manually:
  ```json
  {
    "enabled": true,
    "nickname": "Main",
    "pluginconfigs_path": "C:\\Users\\<you>\\AppData\\Roaming\\XIVLauncher\\pluginConfigs"
  }
  ```

From that path the app derives `AutoRetainer/DefaultConfig.json`, `Lifestream/DefaultConfig.json`, and `XADatabase/xa.db` automatically.

Your settings and chart history are stored **outside** the program folder:

- **Windows:** `%APPDATA%\XA Dashboard`
- **Linux:** `~/.config/XA Dashboard`

So **updating is just replacing the files** — your config and history are preserved.

---

## Settings & version

Everything is configured from the **Settings** page — server host/port, auto-refresh, default theme, highlight colors, item values, build/consumption rates, submarine plans, and the Market Monitor.

The **current version is shown on the Settings page** and via the status endpoint:

```
http://127.0.0.1:21005/api/status   →   { "version": "1.0.0", ... }
```

This release is **v1.0.0**.

---

## Updating

**XA Dashboard updates itself.** It checks aethertek.io for new versions on a schedule you control — Settings → **Updates** offers *every open / daily / weekly / monthly / never* (default: daily). When an update is available, a banner appears with an **Update now** button: one click downloads the new version, verifies its checksum, applies it, and restarts the app. Your settings and chart history are never touched.

Prefer updating by hand? (Linux always updates this way.)

1. Download the new release from https://aethertek.io/applications/xa-dashboard.html.
2. Quit XA Dashboard.
3. Run the installer — or, for portable/Linux installs, unzip over the old folder.
4. Start it again — your settings and chart history carry over automatically.

---

## Data, safety & privacy

- **Read-only:** never writes to your game or to plugin data (`xa.db`, AutoRetainer / Lifestream configs are only read).
- **Local-only:** the web server binds to `127.0.0.1` and serves only your own machine. Nothing is uploaded.
  Charting/export libraries (chart.js, SheetJS) are bundled locally — pages load with **zero** outbound requests, even offline.
  ⚠️ If you change `HOST` to anything other than `127.0.0.1`/`localhost`, the dashboard (all gil/character data and your
  webhook URL) becomes reachable from your network **without a login** — only do that on a network you trust.
- **No telemetry/accounts.** Outbound network calls are limited to the version check against aethertek.io (schedule configurable, can be set to **never**) and the optional Discord Market Monitor webhook, which **you** configure and which is **off by default** (Windows only).
- Use the in-app **Privacy toggle** before sharing screenshots.

---

## FAQ

**Is there a Linux version?**
Yes — a native Linux x64 build is provided. It runs as a headless local web server; open `http://127.0.0.1:21005/` after starting it.

**Can I change the port?**
Yes — Settings → Server, or launch with `--port <number>`. Other flags: `--no-browser`, `--check` (prints a quick data summary and exits).

**Does it modify my game or get me banned?**
No. It only **reads** files your plugins already wrote to disk. It does not touch the game client or memory.

**The Market Monitor / Discord webhook does nothing on Linux.**
Correct — that feature is currently Windows-only. Everything else works on both platforms.

**Is the source code available?**
No. XA Dashboard is proprietary, closed-source software distributed as compiled binaries. See [LICENSE](LICENSE).

---

## Support

Found a bug or have a request? Reach us through the official page at [aethertek.io](https://aethertek.io/applications/xa-dashboard.html) or the community Discord linked there.

---

## License

XA Dashboard is **proprietary software**. The compiled binaries are free to download and use for personal, non-commercial purposes; redistribution, resale, and reverse-engineering are not permitted. See the full [End User License Agreement](LICENSE). Bundled third-party open-source components are listed in [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

> FINAL FANTASY and FINAL FANTASY XIV are registered trademarks of SQUARE ENIX Holdings Co., Ltd. This project is unofficial and not affiliated with or endorsed by SQUARE ENIX.
