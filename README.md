<div align="center">

<img src="assets/icon.svg" width="104" alt="Lerd" />

# Lerd for Windows

> Herd-like local PHP development, native on Windows. Automatic `.test`
> domains, per-project PHP and Node, one-command HTTPS, and no Linux distro
> to look after.

[![Alpha](https://img.shields.io/badge/status-alpha-orange)](#alpha)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-Windows%2010%20%7C%2011-0078d4?logo=windows&logoColor=white)]()
[![Docs](https://img.shields.io/badge/docs-lerd.sh-blue)](https://github.com/lerd-env/lerd/blob/feat/native-windows-integration/docs/getting-started/windows.md)
[![lerd](https://img.shields.io/badge/lerd-lerd.sh-ff2d20)](https://github.com/lerd-env/lerd)
[![Reddit](https://img.shields.io/badge/Reddit-r%2Flerd-ff2d20?logo=reddit)](https://reddit.com/r/lerd)
[![Discord](https://img.shields.io/badge/Discord-Join-5865F2?logo=discord&logoColor=white)](https://discord.gg/5JK54s7xCC)

</div>

[Lerd](https://lerd.sh) is the open-source, rootless local PHP environment for
Linux and macOS: Nginx, PHP-FPM and your services in Podman containers, a web
dashboard, a TUI, a CLI and an MCP server for your AI assistant. This is Lerd
running natively on Windows. You install one `lerd.exe`, it brings its own
container machine, and your sites live in plain Windows folders that your
editor, Git and Explorer already know.

This repository publishes the Windows alpha builds. The code lives on the
[`feat/native-windows-integration`](https://github.com/lerd-env/lerd/tree/feat/native-windows-integration)
branch of [lerd-env/lerd](https://github.com/lerd-env/lerd) and joins the main
release once it is stable.

## Alpha

These builds are for trying Lerd on Windows and telling us what breaks. They are
not ready for daily work yet: queue, schedule and the other framework workers
are still switched off on Windows, see [What is not there yet](#what-is-not-there-yet).
If you need Lerd on Windows today, run it inside
[WSL2](https://lerd.sh/getting-started/wsl2).

## Why native?

- 🪟 **Your files stay in Windows.** Keep projects in `C:\Sites` or anywhere
  else. No second filesystem to copy into, no `\\wsl$` paths in your editor.
- 🧰 **Nothing to set up by hand.** `lerd install` finds or installs Podman,
  turns on Hyper-V or WSL2 for your edition of Windows, and only asks you to
  restart.
- ⚡ **Fast where it is usually slow.** Lerd serves your drives to the machine
  with its own 9p server and drops PHP's opcode cache itself when you save, so
  a Laravel page takes about 0.15 seconds instead of 1.7.
- 🧹 **Clean to remove.** `lerd uninstall` takes away the binary, the login
  entry, the `PATH` entry and the DNS rule.

## Features

- 🌐 **`.test` domains with no hosts file.** A small DNS server built into Lerd
  answers `.test`, and a Windows name resolution rule sends only those names to
  it.
- 🔒 **HTTPS in one command.** `lerd secure` issues a certificate your browsers
  trust.
- 🐘 **PHP and Node per project.** Each site gets its own PHP version, and
  `php`, `composer`, `laravel`, `node` and `npm` shims follow the folder you are
  in, from cmd, PowerShell or Git Bash.
- 🗄️ **Services on tap.** MySQL, PostgreSQL, Redis, Mailpit, Meilisearch, RustFS
  and the rest of the [service store](https://github.com/lerd-env/services),
  wired into each site's `.env` for you.
- 🖥️ **Dashboard, tray and terminal.** The web dashboard opens as its own app
  window, the tray shows what is running, and the site terminal opens in Windows
  Terminal or PowerShell.
- 🔁 **Starts with you.** Lerd comes up when you log in, supervised by its own
  Windows service manager.
- 🧠 **Same Lerd everywhere.** The same CLI, dashboard and
  [framework definitions](https://github.com/lerd-env/frameworks) as on Linux
  and macOS, so a project's `.lerd.yaml` works on every machine on the team.

## Requirements

- Windows 10 or 11, amd64 or arm64.
- Hyper-V (Pro, Enterprise, Education) or WSL2 (any edition, Home included).
  `lerd install` enables whichever your edition supports.
- An elevated PowerShell for the first install, to write the DNS rule and create
  the machine.

## Install

From an elevated PowerShell:

```powershell
irm https://github.com/lerd-env/lerd-windows/releases/latest/download/install.ps1 | iex
```

The script downloads the release for your architecture, checks it against the
release's `checksums.txt` and runs `lerd install`, which copies Lerd into
`%LOCALAPPDATA%\lerd\bin`. If it stops to have you restart Windows, run the
command it prints afterwards. Open a new terminal when it is done, then:

```powershell
cd C:\Sites\my-app
lerd link
lerd secure
```

To update, run `lerd update`. `lerd update --rollback` goes back to the version
you had before.

## What is not there yet

- **Workers.** Queue, schedule, Horizon and the other framework workers are
  switched off on Windows for now.
- **Tinker autocomplete.** One of the bundled tools has no Windows build wired in
  yet.
- **Path mapping** between Windows and the machine has had little testing on
  either Hyper-V or WSL2.

The full picture, with how each piece works, is in the
[Windows guide](https://github.com/lerd-env/lerd/blob/feat/native-windows-integration/docs/getting-started/windows.md).

## Feedback

Found something broken? Open an issue on
[lerd-env/lerd](https://github.com/lerd-env/lerd/issues) and mention Windows
and the alpha version from `lerd --version`, or tell us on
[Discord](https://discord.gg/5JK54s7xCC).

## Remove it

```powershell
lerd uninstall
podman machine rm -f
```

## License

MIT, see [LICENSE](LICENSE).
