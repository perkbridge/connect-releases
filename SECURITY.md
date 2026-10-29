# Security policy

## Reporting a vulnerability

Please report vulnerabilities privately to **support@perkbridge.eu**, not in
a public issue. Include the version, what you found, and how to reproduce it.

## Supported versions

| Version | Supported |
|---|---|
| 1.0.0-beta | Yes |

## Verifying releases

Every release is signed with the key in [`allowed_signers`](allowed_signers).
See [README.md](README.md#verify-a-download). A file that does not verify is
not from PerkBridge: delete it and tell us where you got it.

## Key changes

A new release key is announced in this repository at least one release
before it signs anything. A revoked key is removed from
`allowed_signers` and from every new release.
