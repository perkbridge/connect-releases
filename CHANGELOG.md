# Changelog

All notable changes to PerkBridge Connect. Versions follow
[Semantic Versioning](https://semver.org/).

## [1.0.0-beta] - 2026-10-29

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

[1.0.0-beta]: https://github.com/perkbridge/connect-releases/releases/tag/v1.0.0-beta
