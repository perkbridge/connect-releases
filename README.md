# PerkBridge Connect releases

Signed releases of **PerkBridge Connect**, the self-hosted platform that runs
and supervises MetaTrader 5 terminals on your own Ubuntu servers.

This repository holds only signed release files, the release key, the
changelog and the security policy. Download a release from
[Releases](https://github.com/perkbridge/connect-releases/releases); see
[CHANGELOG.md](CHANGELOG.md) for what changed.

| | |
|---|---|
| Latest | **1.0.0-beta** (pre-release) |
| Servers | Ubuntu 24.04 LTS, amd64 |
| Desktop installer | macOS, Windows, Ubuntu |
| Release key | `perkbridge-release-1` |

## Features

- **Your servers, your data.** Connect your own Ubuntu VPS or dedicated
  servers. Nothing runs on PerkBridge infrastructure.
- **One-click server setup.** Onboard a server over SSH with host-key
  confirmation and temporary credentials; Docker, the Fleet Agent, the
  watchdog and the credential vault are installed for you.
- **Isolated MetaTrader 5 terminals.** Each deployment runs in its own Docker
  container under Wine, with memory limits, restart after reboot and a
  guided create flow.
- **Keeps trading when the dashboard is away.** Terminals keep running if the
  Control Plane is unreachable; the agent and watchdog work locally.
- **Failover.** A hot standby on a second server, armed from the dashboard,
  with safety rules that prevent duplicate trading.
- **Credentials never in clear.** Broker passwords live in a split-key vault on
  the trading server.
- **Trading controls.** Accounts, orders, risk rules (limits, trading hours,
  news blackout) and an emergency stop. Closing positions always stays
  possible.
- **Trading API.** REST API with scoped API tokens.
- **Monitoring and alerts.** Server and terminal health, events and alerts,
  delivered to Telegram, Slack, email or signed webhooks.
- **PerkBridge Identity.** Optional single sign-on with users, groups and
  roles.
- **Signed updates.** Agent and host components update from the dashboard;
  every update is signed and verified on the server before it runs.
- **Audit log** of sensitive actions.

## Install

1. Download the `nile-setup` file for your computer, plus `SHA256SUMS`,
   `SHA256SUMS.sig` and `allowed_signers`, from the latest release.
2. Verify them (below).
3. Run `nile-setup`, enter your server's address and SSH sign-in, and follow
   the seven screens. It downloads the bundle to the server, verifies its
   signature there, and installs everything.

Prefer the command line? Download the bundle on the server, verify it, and run
`sudo nile-VERSION/nile-install` for a guided install.

## Release key

Every release is signed with the PerkBridge release key. It is the file
[`allowed_signers`](allowed_signers) in this repository:

```text
release@nile.perkbridge namespaces="install@nile.perkbridge,setup@nile.perkbridge" ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILSNt1/bOrvWtbWGkdXYPh6k9r8YhRCSUbKNIrYTlsQL
```

| Key ID | Since |
|---|---|
| `perkbridge-release-1` (fingerprint `SHA256:ie95DCAUWc6nVeo5PPGo9OHZKw36ECE1mA0213OSfns`) | 1.0.0-beta |

Take the key from here, never from inside a download. A release that does
not verify against this key is not from PerkBridge.

## Verify a download

To check the desktop installer (`nile-setup-*`) and the checksums:

```sh
ssh-keygen -Y verify -f allowed_signers -I release@nile.perkbridge \
  -n setup@nile.perkbridge -s SHA256SUMS.sig < SHA256SUMS
sha256sum --ignore-missing -c SHA256SUMS   # macOS: shasum -a 256 --ignore-missing -c SHA256SUMS
```

To check the server bundle, after `tar -xzf nile-VERSION-linux-amd64.tar.gz`:

```sh
ssh-keygen -Y verify -f allowed_signers -I release@nile.perkbridge \
  -n install@nile.perkbridge -s nile-VERSION/install-manifest.json.sshsig \
  < nile-VERSION/install-manifest.json
nile-VERSION/nile-install verify --allowed-signers allowed_signers
```

Each `ssh-keygen` command must print `Good "…" signature for release@nile.perkbridge`.

`nile-setup` carries the same key compiled in and runs these checks on the
server for you.

## Requirements

- Trading servers: Ubuntu 24.04 LTS, amd64, with root access.
- Desktop installer: macOS (Apple silicon or Intel), Windows 10/11 x64, or
  Ubuntu x64/arm64.

## Licence

PerkBridge Connect is commercial software. Each release carries a signed
default licence; larger licences are issued by PerkBridge. MetaTrader 5 is
not included and is downloaded from MetaQuotes on your server.
