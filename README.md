# Keep rotator

Catalog entry for `keep-rotator`, the Mesh extension that lets
[Keep](https://github.com/entire-vc/evc-keep) rotate Mesh agent keys and its own
Mesh token. This repository holds only the manifest: there is no code here. The
service behind it is the Keep server you already run (`runtime: external`).

Keep connects to Mesh with two extensions. `keep` (installed from the Keep
repository) confirms actions through Mesh cards, mirrors the Mesh hierarchy and
shows the secrets screen. `keep-rotator` holds only the rotation rights, so a
leaked token of one cannot do the work of the other. See
[Keep as a Mesh extension](https://github.com/entire-vc/evc-keep/blob/main/docs/mesh-extension.md)
and the [service account](https://github.com/entire-vc/evc-keep/blob/main/docs/mesh-service-account.md).

## Permissions

- `agent_keys:read`: read agent keys to find the ones due for rotation.
- `agent_key:rotate`: issue a new key for an agent and retire the old one.
- `ext_token:self_rotate`: replace this extension's own Mesh token.

No events, no screens, no secrets, no network domains.

## Settings

- `keep_url` (instance, required): the address of your Keep server.

## Network

Mesh calls only the Keep server at `keep_url` (`/healthz`, `/mesh/install`,
`/mesh/events`). For this extension Keep calls only the Mesh instance it is connected to.
Nothing else is contacted.

## Accounts

None beyond your own. The extension acts as one Mesh service actor per
workspace; its first token is issued in Mesh to the administrator and handed to
Keep, which seals it and rotates it from then on. No third-party account is
created or required.

## Telemetry

None. Neither Keep nor this extension sends usage data anywhere. Key rotations
are recorded only in Keep's own audit journal.

## Source of truth

The manifest is maintained in the Keep repository as
`mesh-extension.keep-rotator.json`; `mesh-extension.json` here is a byte-for-byte
copy of it at the matching version. Change it there first, then copy it here and
tag the same version.

## License

Apache-2.0, see [LICENSE](LICENSE).
