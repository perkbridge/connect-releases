# Changelog

All notable changes to PerkBridge Connect. Versions follow
[Semantic Versioning](https://semver.org/).

## [1.0.3-beta] - 2026-10-30

### Added

- Edit SL/TP on an open position (dashboard and API). During a halt, or with
  trading or the route off, only a missing stop can be added. A lost answer
  ends as "answer lost" with the prior levels kept for review.
- Start a stopped or failed deployment as a new session on the same server;
  Create again after removal.
- Revert a runtime image update to the previous image.
- MT5 runtime `nile-mt5-runtime` main-be088e5 (Wine 11.20) with Edit SL/TP.

### Changed

- Remembered credentials are on by default for new servers; the
  installation's administrator can trade on their own accounts without
  single sign-on; risk mode can be chosen when a deployment is created.
- Release sequence 5. `nile-install upgrade` installs it over 1.0.2-beta (4),
  1.0.1-beta (3), 1.0.0-beta (2) and installations from before release
  sequences. It changes the database, so going back means restoring the
  upgrade's backup.

## [1.0.2-beta] - 2026-10-29

### Added

- Settings shows the organization's licence (limits and expiry).

### Fixed

- The administrator created by `nile-install` lacked notification, audit,
  failover and trading administration rights on the organization.
- A finished MetaTrader 5 image download could be reported as failed until
  the server's next report; it is now shown as present at once. New servers
  allow 2 hours for an image download.

### Changed

- Release sequence 4. `nile-install upgrade` installs it over 1.0.1-beta
  (3), 1.0.0-beta (2) and installations from before release sequences.

## [1.0.1-beta] - 2026-10-29

Replaces 1.0.0-beta.

### Fixed

- Adding a server from the dashboard was refused with "the release manifest
  lists no runtime image, so no host policy can be generated": the 1.0.0-beta
  release manifest named no MetaTrader 5 runtime image. 1.0.1-beta names
  `ghcr.io/perkbridge/nile-mt5-runtime` main-6091472 (Wine 11.20) by digest.

### Changed

- Release sequence 3. `nile-install upgrade` installs it over 1.0.0-beta
  (sequence 2) and over installations from before release sequences.

## 1.0.0-beta - 2026-10-29 (withdrawn)

First public beta.

### Added

- **Installation:** the `nile-setup` desktop installer for macOS, Windows and
  Ubuntu, and the `nile-install` server installer with guided and one-command
  modes, preflight checks, optional Docker installation, backup, restore and
  `upgrade` with automatic backup.
- **Release signing:** every bundle, its installation manifest, the agent
  release and the `SHA256SUMS` of every asset are signed with the PerkBridge
  release key `perkbridge-release-1`.
- **Servers:** SSH onboarding with host-key confirmation, the Fleet Agent,
  watchdog and nile-vault installed per server, telemetry, and decommissioning.
- **Deployments:** MetaTrader 5 terminals in Docker under Wine (Wine 11.20),
  created through a guided journey (platform, account, server), with start,
  stop, restart after reboot (on by default), memory limits, and a runtime
  catalogue with signed image metadata.
- **Failover:** hot standby on a second server, armed from the dashboard, with
  promotion rules that prevent duplicate execution.
- **Credentials:** split-key nile-vault on each trading server, remembered
  credentials, and vault-to-vault transfer for failover.
- **Trading:** accounts, orders, risk rules (limits, trading hours, news
  blackout), routes, strategies and an emergency stop. Closing positions and
  cancelling orders are always allowed, even during a halt.
- **Trading API:** a REST API (OpenAPI 3.1) with API tokens.
- **Monitoring:** health, events, alerts, deployment alerts and a Control Plane
  view.
- **Notifications:** Telegram, Slack, email and signed webhooks through
  nile-notifier, with rules and a delivery log.
- **PerkBridge Identity:** optional single sign-on with users, groups and roles;
  can be enabled on an existing installation.
- **Platform updates:** signed agent and host updates from the dashboard
  (release sequence 2).
- **Licensing:** signed licences with server, deployment, terminal and channel
  limits. The bundle carries a default licence (1 server, 2 deployments,
  1 notification channel, valid until 2027-10-29). Without a licence a new
  installation gets the same starter allowance.

### Known limitations

- Trading servers: Ubuntu 24.04 on amd64 only. Windows servers are planned.
- Some MetaTrader 5 builds detect Wine's debugger layer and refuse to start.
- Desktop installers aren't notarized by Apple or code-signed by Microsoft.
- No self-service licence portal yet.

[1.0.3-beta]: https://github.com/perkbridge/connect-releases/releases/tag/v1.0.3-beta
[1.0.2-beta]: https://github.com/perkbridge/connect-releases/releases/tag/v1.0.2-beta
[1.0.1-beta]: https://github.com/perkbridge/connect-releases/releases/tag/v1.0.1-beta
