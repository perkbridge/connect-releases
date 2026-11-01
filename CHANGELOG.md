# Changelog

All notable changes to PerkBridge Connect. Versions follow
[Semantic Versioning](https://semver.org/).

## [1.1.0-beta] - 2026-11-01

### Added

- Orders › Closed trades reads MT5 trade history directly from the running
  terminal, including trades placed outside PerkBridge Connect. Each trade
  includes its broker deals and the result after commission, swap and fees;
  matching platform orders supply the route and strategy. Browse 10 trades
  at a time with Newer and Older.
- Accepted DirectConnect architecture (ADR-045) and an experimental benchmark
  kit in the source repository. The production DirectConnect order path is
  not implemented or available in this release.

### Changed

- MT5 history is read from the terminal on each request, without storing a
  copy of the broker's history. Positions and exposure return a
  terminal-not-running error when the terminal is stopped; the dashboard
  shows a notice instead of presenting the last reported positions as current.
- Update trading servers' host components from the dashboard: live history
  requires the updated Fleet Agent. Update deployment runtime images for
  the calendar-value fix below.
- No new database migrations compared with 1.0.5-beta. Rollback to that
  version does not require a database restore. For older versions, follow
  the install guide's rollback requirements.

### Fixed

- Malformed or overlong broker event names no longer reject an entire
  calendar-value response and leave news rules unable to decide. Names are
  cleaned for display; rules still identify events by ID.
- Server cleanup recognises newer agent state, including placement records,
  catalogue caches, risk directories and the trade ledger. Unforwarded risk
  events and unresolved order outcomes still block cleanup, including
  unknown outcomes already marked finished locally.

### Known limitations

- MT5 history requires a running terminal with its history loaded. A read
  can take up to 30 seconds and reports an error when history is temporarily
  unavailable. Broker position-ID assumptions and netting-reversal handling
  have not yet been verified against a real terminal.

## [1.0.5-beta] - 2026-10-31

### Fixed

- Time rules plan again after a terminal restart, reboot or failover switch
  inside a time block; before, positions and pending orders the rule should
  close or cancel could stay open until the block ended.
- Edit SL/TP on MT5 positions checks the broker's order modes instead of
  refusing with "stops not supported".
- Host cleanup is no longer refused by earlier finished sessions of a
  deployment started more than once.

### Added

- Settings › Licence: download the activation request and upload a licence
  file (the installation's first administrator owns the licence).
- nile-setup updates an installed management server to a newer release; a
  failed update returns to the previous release.
- nile-setup errors link to the troubleshooting page.
- MT5 runtime `nile-mt5-runtime` main-a3ffb64 (Wine 11.20) with the time
  rules fix.

### Changed

- Release sequence 7. `nile-install upgrade` installs it over 1.0.4-beta (6)
  and 1.0.3-beta (5) with a binary rollback, and over 1.0.2-beta (4),
  1.0.1-beta (3), 1.0.0-beta (2) and installations from before release
  sequences with `--rollback-restore`. No database changes.

## [1.0.4-beta] - 2026-10-31

### Fixed

- An economic calendar event with an empty or badly encoded name no longer
  breaks the whole calendar; the news picker and news rules load again and
  list such an event by its id.
- The credential vault starts after a reboot on servers added with earlier
  betas: the updater repairs narrower state and slot folder permissions at
  start and never narrows a wider mode.
- A market close rule covering all symbols reports ready after a restart
  with nothing open, instead of refusing opening orders.

### Added

- MT5 runtime `nile-mt5-runtime` main-51bd371 (Wine 11.20) with the
  calendar and market close fixes.
- Deployed rules tab on the Risk page; failover history shows the latest 5
  steps with older ones on demand.

### Changed

- Release sequence 6. `nile-install upgrade` installs it over 1.0.3-beta (5),
  with a binary rollback, and over 1.0.2-beta (4), 1.0.1-beta (3),
  1.0.0-beta (2) and installations from before release sequences with
  `--rollback-restore`. No database changes.

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

[1.1.0-beta]: https://github.com/perkbridge/connect-releases/releases/tag/v1.1.0-beta
[1.0.5-beta]: https://github.com/perkbridge/connect-releases/releases/tag/v1.0.5-beta
[1.0.4-beta]: https://github.com/perkbridge/connect-releases/releases/tag/v1.0.4-beta
[1.0.3-beta]: https://github.com/perkbridge/connect-releases/releases/tag/v1.0.3-beta
[1.0.2-beta]: https://github.com/perkbridge/connect-releases/releases/tag/v1.0.2-beta
[1.0.1-beta]: https://github.com/perkbridge/connect-releases/releases/tag/v1.0.1-beta
