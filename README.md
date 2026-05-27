![Windows](https://img.shields.io/badge/platform-Windows-blue)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# AI Usage Monitor

![Screenshot](.github/animation.gif)

A lightweight Windows floating desktop widget for people already using Claude Code, with optional ChatGPT/Codex usage display.

It sits on your desktop and shows how much of your Claude and/or ChatGPT usage window you have left, without needing to open the terminal or the provider site.

## What You Get

- A **5h** bar for your current 5-hour Claude usage window
- A **7d** bar for your current 7-day window
- Optional ChatGPT/Codex usage bars alongside Claude
- A live countdown until each limit resets
- A small native floating widget that can be placed anywhere on the Windows desktop
- Optional Always on Top mode
- A single generic system tray icon
- Double-click the tray icon to toggle the floating widget on or off
- Right-click options for refresh, displayed models, update frequency, language, startup, and updates

## Who This Is For

This app is for Windows users who already have **Claude Code (CLI or App) installed and signed in**.

Codex support is optional. To show Codex usage, install and sign in to the Codex CLI, then enable Codex from the right-click **Models** menu.

It works best if you want a simple "how close am I to the limit?" display that is always visible.

## Requirements

- Windows 10 or Windows 11
- Claude Code (CLI or App) installed and authenticated
- Optional: Codex CLI installed and authenticated, if you want Codex usage

If you use Claude Code through WSL, that is supported too. The monitor can read your Claude Code credentials from Windows or from your WSL environment.

## Install

Download the latest `ai-usage-monitor.exe` from the [Releases](https://github.com/jpribil/AI-Usage-Monitor/releases) page and run it directly.

## Use

After installing with WinGet, run:

```powershell
ai-usage-monitor
```

Once running, it will appear as a floating desktop widget and as a tray icon in the notification area.

- Drag the widget to move it around the desktop
- Right-click the floating widget or tray icon for refresh, displayed models, update frequency, Start with Windows, reset position, language, updates, and exit
- Enable `Always on Top` from the right-click Settings menu if you want it to stay above other windows
- Double-click the tray icon to toggle the floating widget on or off
- Enable `Start with Windows` from the right-click menu if you want it to launch automatically when you sign in

### Models

Use the right-click **Models** menu to choose what the widget displays:

- **Claude Code** is enabled by default
- **Codex** can be enabled alongside Claude Code or shown by itself

When both models are shown, each model has its own usage bar and matching usage text color.

### System Tray Icon

The app shows one generic tray icon. Double-click it to show or hide the floating widget, or right-click it for the app menu.

## Diagnostics

If you need to troubleshoot startup or visibility issues, run:

```powershell
ai-usage-monitor --diagnose
```

This writes a log file to:

```text
%TEMP%\ai-usage-monitor.log
```

Settings are saved to:

```text
%APPDATA%\AIUsageMonitor\settings.json
```

## Account Support

This app works with the same account types that Claude Code itself supports.

As of **March 19, 2026**, Anthropic's Claude Code setup documentation says:

- **Supported:** Pro, Max, Teams, Enterprise, and Console accounts
- **Not supported:** the free Claude.ai plan

If Anthropic changes Claude Code availability in the future, this app should follow whatever Claude Code supports, as long as the usage data remains exposed through the same authenticated endpoints.

## Privacy And Security

This project is **open source**, so you can inspect exactly what it does.

What the app reads:

- Your local Claude Code OAuth credentials from `~/.claude/.credentials.json`
- If needed, the same credentials file inside an installed WSL distro
- If Codex is enabled, your local Codex credentials from `$CODEX_HOME/auth.json` or `~/.codex/auth.json`

What the app sends over the network:

- Requests to Anthropic's Claude endpoints to read your usage and rate-limit information
- Requests to ChatGPT's Codex usage endpoint to read your Codex usage and rate-limit information, if Codex is enabled
- Requests to GitHub only if you use the app's update check / self-update feature
- If proxy environment variables such as `HTTPS_PROXY`, `HTTP_PROXY`, or `ALL_PROXY` are set, those outbound requests may use that proxy

What the app stores locally:

- Widget position
- Polling frequency
- Language preference
- Last update check time
- Displayed model preferences

What it does **not** do:

- It does not send your credentials to any other server
- It does not use a separate backend service
- It does not collect analytics or telemetry
- It does not upload your project files
- It does not directly edit your Codex credentials file

Notes:

- If your Claude Code token is expired, the app may ask the local Claude CLI to refresh it in the background
- If your Codex token is expired, the app may ask the local Codex CLI to refresh it in the background. The monitor does not write `auth.json` itself; any credential update is handled by the Codex CLI.
- Portable installs can update themselves by downloading the latest release from this repository
- Proxies should be trusted because proxied usage requests include your OAuth bearer token inside the TLS connection

## How It Works

The monitor:

1. Finds your enabled model login credentials
2. Reads your current usage from Anthropic and/or ChatGPT
3. Shows the result in a floating Windows desktop widget
4. Refreshes periodically in the background

If the newer usage endpoint is unavailable, it can fall back to reading the rate-limit headers returned by Claude's Messages API.

## Open Source

This project is licensed under MIT.

If you want to inspect the behavior or audit the code, everything is in this repository.
