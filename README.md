<div align="center">

# RemoteX

### Tell RemoteX what is broken. It diagnoses, proposes a fix, runs the commands you approve, and verifies the result — across SSH servers, Kubernetes clusters, and databases.

An AI command center for the infrastructure you already operate — for macOS, Linux, Windows, iPhone, and Android. Keys never leave your device.

[**⬇ Mac (Apple Silicon)**](https://github.com/meetprimo/remotex/releases/latest/download/RemoteX-arm64.dmg) &nbsp;·&nbsp; [**⬇ Linux AppImage**](https://github.com/meetprimo/remotex/releases/latest/download/RemoteX.AppImage) &nbsp;·&nbsp; [**⬇ Linux .deb**](https://github.com/meetprimo/remotex/releases/latest/download/RemoteX-x64.deb) &nbsp;·&nbsp; [**⬇ Windows installer**](https://github.com/meetprimo/remotex/releases/latest/download/RemoteX-Setup.exe) &nbsp;·&nbsp; [Release notes](https://github.com/meetprimo/remotex/releases/latest) &nbsp;·&nbsp; [Discord](https://discord.gg/fb4X6kxtAH) &nbsp;·&nbsp; [remotex.dev](https://remotex.dev)

[![Download on the App Store](https://img.shields.io/badge/iPhone-App%20Store-000?logo=apple)](https://apps.apple.com/us/app/remotex-server-ops/id6766106493) [![Get it on Google Play](https://img.shields.io/badge/Android-Google%20Play-000?logo=googleplay)](https://play.google.com/store/apps/details?id=dev.remotex.app)

<br>

<sub>BRING YOUR OWN AI · WORKS WITH</sub>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/provider-strip-dark.svg">
  <img alt="Supported AI providers: Anthropic, OpenAI, Gemini, GitHub Copilot, OpenRouter, xAI, DeepSeek, Mistral, Vercel, Amazon Bedrock, Ollama, LM Studio" src="assets/provider-strip-light.svg" width="720">
</picture>

<br>

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/app-dark.png">
  <img alt="RemoteX desktop app — multi-connection sidebar, a chat-first conversation diagnosing a production issue, command output cards, and a Touch-ID-gated approval card." src="assets/app-light.png" width="900">
</picture>

</div>

---

## What it is

RemoteX is an AI agent that operates the infrastructure you already manage. Describe a problem in plain English — *"orders table is slow this morning"*, *"the staging pod won't start"*, *"disk on /var keeps filling up"* — and it runs read-only checks, reads logs, queries databases, inspects Kubernetes, proposes a concrete fix, runs the commands **you approve**, and verifies the result. Full SSH client, K8s client, and database client all in one chat-first surface.

This repository hosts the signed macOS builds, the Linux AppImage and `.deb`, the Windows installer and portable `.exe` (unsigned preview), and the auto-update feed. **Source code is private**; this is the public home for downloads, releases, and feedback.

## Features

### Three kinds of connection, one agent

- **SSH servers.** Linux, BSD, macOS, Synology, embedded. Full SSH client with SFTP file viewer and a real interactive PTY terminal alongside the chat.
- **Kubernetes clusters.** Add a cluster by pasting a kubeconfig — token, client-cert, or exec auth. The agent talks to the apiserver directly: `kubectl`-equivalent ops, pod logs, exec into containers, describe and diff manifests.
- **Databases.** First-class Postgres, MySQL, Redis, and SQLite. Read schemas, ask analytical questions in English, run scoped queries with row caps, redacted credentials, and an approval gate on anything that mutates.
- **SSH bastion tunneling.** Databases and Kubernetes clusters that only live behind a jump host? Attach the bastion to the connection — RemoteX opens a tunnel automatically, and the agent understands the relationship ("this database is *reached through* server X, they are not the same machine"). No more confusing the DB with its host.

### The agent loop

- **AI agent, not autocomplete.** Describe the problem in plain English. It runs read-only diagnostics autonomously, reads logs, queries DBs, inspects K8s — diagnose → propose → run → verify.
- **Plan mode.** Turn it on when you want the agent to propose the investigation first and wait for approval. Once approved, one live checklist stays on screen and ticks forward as each step starts and finishes, with `Working for` / `Worked for` timing.
- **Readable runs.** The agent often runs several read-only commands or queries to answer one question. Each lands as a labeled card under a collapsible "Ran N commands" header — expand any output, copy the raw bytes, or jump back into the agent's reasoning with it all pre-loaded.
- **App-native slash commands.** `/help`, `/model`, `/summarize`, `/memorize`, and more.

### Approvals & safety

- **You approve every change.** Mutating commands, kubectl writes, and DB write-queries always surface as a card with the exact bytes about to run, a plain-English explanation, and a risk label.
- **Biometric or local gate, every time.** On macOS the Approve & run button is gated by Touch ID; on Linux a local PIN/password gate stands in. Every time, no exceptions, even for a command you typed yourself. The agent never has standing access.
- **Read-side guardrails.** Database queries enforce row limits, redact sensitive columns, and label every result with its source connection so multi-DB sessions stay legible.

### Files & code

- **Attach files to chat.** Drop PDFs, Word, Excel, PowerPoint, CSV, logs, or source code into the composer. They render as chips on the message, are sent natively to models that support documents (Claude, Gemini), and are extracted to text for models that don't — same UX on macOS, Linux, iPhone, and Android.
- **Remote file viewer.** A two-pane viewer in the right rail: collapsible tree plus a syntax-highlighted preview with line numbers and a copy button — shell, Python, JS/TS, JSON, YAML, nginx, dotfiles, Dockerfile. Uses SFTP when available, with a read-only SSH fallback on servers like Synology where SFTP is disabled.
- **Open in your editor, safely.** Double-click a remote file and RemoteX hands it to your associated editor. On save it detects the change, diffs it against the original, and surfaces an approval card with the exact patch before writing back over SFTP.
- **Real terminal.** A bottom-docked PTY opens an interactive shell over the same SSH connection the agent uses — run vim, an interactive installer, or a live tail without burning agent context. Dock it below the chat or float it right.
- **Activity stream — one continuous flow of every command and its output.** Sit next to the agent in a real `script(1)`-style terminal: each command lands as `$ command` with its actual stdout/stderr beneath it, in equal-weight monospace with a green prompt. No chat noise, no synthesis, no nested scrollboxes — just one terminal scrollback you can scroll through end-to-end. Type your own shell commands or SQL queries directly into the green `❯` prompt at the bottom: they execute in ~100ms without an LLM round-trip, and the agent **reads them as context on your next turn** — so anything you ran by hand is fully visible to the agent the moment you ask it a follow-up question. The bridge works both ways: agent-issued commands appear in your scrollback, your direct-exec commands appear in agent context. Live monitoring footer (CPU / mem / disk / load) pinned at the bottom.

### Context inputs

- **Send a screenshot.** Paste or drop an image — a metrics panel, a stack trace, an error page, even a photo of a monitor — passed only to vision-capable models as visual context.
- **Local memory.** Recalled automatically per connection, visible and removable per entry from the right rail.

### Fleet & recall

- **Multi-connection, chat-first.** A sidebar of per-connection chats with command-output cards, stop/retry, and titles drawn from your first question.
- **Pinned panel.** Star any message — the agent's synthesis, a command output, your own question — and it appears in a dedicated Pinned panel across every connection. Pins travel with your encrypted backup.
- **Search across the fleet.** Find the thing you need across servers, clusters, and databases; archived chats are searchable with restore support.

### Automate & watch

- **Snippets & playbooks.** Save plain-English intents and chain multi-step routines with approval gates, reusable across every connection.
- **Monitoring & alerts.** Per-server rules sample CPU, memory, disk, and services natively over SSH and notify you before a customer does.

### Bring your own AI

- **14 providers, your bill.** Sign in with ChatGPT or a Copilot seat, or use API keys for Anthropic, Gemini, Mistral, OpenRouter, xAI, DeepSeek, and more — including local models (Ollama, LM Studio). Switch per chat.
- **Never proxied.** Tokens live in the OS secure store (macOS Keychain / libsecret on Linux); traffic goes straight to the provider. No RemoteX backend in the connection path.

### Privacy

- **Keys stay on-device.** SSH keys, kubeconfigs, database passwords, and AI tokens live in the OS secure store. Direct-to-target, biometric gates, no cloud relay.
- **Encrypted backups.** Round-trip backups bundle everything — servers, clusters, databases, AI keys, snippets, playbooks, pins — encrypted with a passphrase you choose.

### Platforms & pricing

- **Mac, Linux, Windows, iPhone, Android.** One subscription covers all of them. **Free** covers one connection and one of each snippet/playbook/rule; **Pro** removes the caps.
- **Signed & auto-updating.** Every macOS build is signed with an Apple Developer ID and notarized. The Linux AppImage embeds its own update logic and tracks the same release feed; the `.deb` carries a desktop entry under Applications and auto-updates via the AppImage updater. Windows is currently an **unsigned preview** — SmartScreen may warn on first run; auto-update works via the NSIS installer once installed. Install once and RemoteX updates itself silently in the background.

## Install

### macOS (Apple Silicon)

```
https://github.com/meetprimo/remotex/releases/latest/download/RemoteX-arm64.dmg
```

Or via the site, which redirects to the same asset:

```
https://remotex.dev/api/download?platform=mac
```

Apple Silicon only for now — Intel builds are not published yet.

### Linux (x86_64)

**AppImage** — works on any modern distro, no install needed:

```sh
wget https://github.com/meetprimo/remotex/releases/latest/download/RemoteX.AppImage
chmod +x RemoteX.AppImage
./RemoteX.AppImage
```

**Debian / Ubuntu / Mint** — install as a system package:

```sh
wget https://github.com/meetprimo/remotex/releases/latest/download/RemoteX-x64.deb
sudo dpkg -i RemoteX-x64.deb
sudo apt-get install -f   # if dependencies are missing
```

Or via the site:

```
https://remotex.dev/api/download?platform=linux-appimage
https://remotex.dev/api/download?platform=linux-deb
```

### Windows (x64, unsigned preview)

The first Windows builds ship **unsigned** on purpose — see the [signing rationale](#why-the-windows-build-is-unsigned-for-now) below. Expect a `Windows protected your PC` SmartScreen prompt on first launch; click **More info → Run anyway**.

**Installer (NSIS)** — adds a Start menu entry, registers an uninstaller, and auto-updates via electron-updater:

```powershell
iwr https://github.com/meetprimo/remotex/releases/latest/download/RemoteX-Setup.exe -OutFile RemoteX-Setup.exe
.\RemoteX-Setup.exe
```

**Portable single `.exe`** — no installer, drop it anywhere:

```powershell
iwr https://github.com/meetprimo/remotex/releases/latest/download/RemoteX-x64.exe -OutFile RemoteX.exe
.\RemoteX.exe
```

Or via the site:

```
https://remotex.dev/api/download?platform=windows-installer
https://remotex.dev/api/download?platform=windows-portable
```

Also available on [iPhone](https://apps.apple.com/us/app/remotex-server-ops/id6766106493) and [Android](https://play.google.com/store/apps/details?id=dev.remotex.app).

### Why the Windows build is unsigned (for now)

Modern Windows SmartScreen treats EV, OV, and Azure Trusted Signing certificates through the same reputation model — paying for a cert without download volume to build reputation is wasted spend. The plan:

1. **Ship unsigned now** (this is what you're downloading) and let SmartScreen reputation accumulate organically as the download count grows.
2. **Add MSIX via the Microsoft Store** as the trusted-install path — Microsoft re-signs MSIX packages automatically, no cert cost on our side, no SmartScreen warning for the Store version.
3. **Revisit Azure Artifact Signing** ($10/month) for direct `.exe` downloads only when Windows usage proves out.

If the SmartScreen prompt blocks you and you'd rather not click through, the Microsoft Store path will be the right one once it ships.

## Verify the build

### macOS

```sh
# Verify the DMG is signed and notarized:
spctl -a -vvv -t install /path/to/RemoteX-arm64.dmg

# After installing, verify the .app:
codesign -dv --verbose=4 /Applications/RemoteX.app
```

A correctly notarized build prints `source=Notarized Developer ID` and shows a valid `Developer ID Application: <Name> (TEAMID)` identity.

### Linux

The AppImage and `.deb` are built reproducibly inside the `electronuserland/builder:22` container. Check the embedded version and maintainer:

```sh
# .deb metadata (package name, version, maintainer):
dpkg-deb -I RemoteX-x64.deb

# AppImage embedded version + offset (extracts to a temp directory):
./RemoteX.AppImage --appimage-extract-and-run --version
```

### Windows

The Windows binaries are **unsigned** for now — there is nothing to verify with `signtool verify`. Cross-check the SHA-256 against the value published on the GitHub release:

```powershell
Get-FileHash RemoteX-Setup.exe -Algorithm SHA256
Get-FileHash RemoteX-x64.exe   -Algorithm SHA256
```

Compare each hash with the corresponding entry in the `latest.yml` asset on the release page. Once we move to MSIX via the Microsoft Store, the Store-distributed copy will carry Microsoft's signature automatically.

## Auto-update

RemoteX checks this repo for new releases on launch and roughly once an hour while running. Updates download in the background and install on next launch. Change or disable the update channel under **Settings → Updates** in the app. Beta-channel users automatically receive stable releases too — whichever is newer wins. The Linux AppImage uses electron-updater against `latest-linux.yml`; `.deb` installs share the same updater because the AppImage payload is embedded. The Windows installer auto-updates against `latest.yml`; the **portable `.exe`** does not auto-update (replace the file manually when a new release ships).

## Issues and feedback

Bug reports and feature requests are welcome in [Issues](https://github.com/meetprimo/remotex/issues), [Discussions](https://github.com/meetprimo/remotex/discussions), and the [RemoteX Discord](https://discord.gg/fb4X6kxtAH), or via the in-app feedback flow under **Settings → About** and at <https://remotex.dev/contact>.

## License

Binaries published here are governed by the [RemoteX Terms of Use](https://remotex.dev/terms) that ship with the app. The repository itself is not separately open-source-licensed.
