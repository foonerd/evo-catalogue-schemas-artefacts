# evo-catalogue-schemas-artefacts

> The release plane for [evo-catalogue-schemas](https://github.com/foonerd/evo-catalogue-schemas). Signed catalogue-schema artefacts, fetched by any distribution or plugin build.

Manifest in. Signed bytes out. Consumers pick the channel.

This repository is the device-facing (and distribution-facing) surface of the catalogue schemas that pin every shelf shape, verb envelope, and subject contract in the ecosystem. Editing source in [evo-catalogue-schemas](https://github.com/foonerd/evo-catalogue-schemas) does not touch these assets. What lands here is exactly what a plugin build or distribution fetches and verifies at catalogue-load time.

## What lives here

- `channels/` — per-channel signed pointer TOML files (`dev.toml`, `test.toml`, `prod.toml`). Each pointer names the current schema-set version for that channel.
- `bundles/` — signed schema-set artefacts (one tarball per released version) containing the complete `org.evoframework/**/*.toml` schema tree at that version. Bit-identical across channels.
- `LICENSE` — Apache-2.0 for the assets in this repo.

## Channels

Three named tracks of release readiness: `dev`, `test`, `prod`. Same shape as every other release plane in the evo ecosystem.

- **A channel is a pointer, not a bucket.** A schema-set version is built once, signed once, stored once. Promotion from `dev` to `test` is a pointer edit — the channel now names that version. The bytes do not change; the signature does not change.
- **Selection is per-consumer.** A plugin build or distribution names which channel's pointer to track for its schema baseline.

## Signing

Every artefact under `bundles/` is signed against the evo project release key. Every pointer TOML under `channels/` is signed against the same key. Consumers verify signatures before applying.

## Consumer contract

Fetch `channels/<channel>.toml`, verify signature, resolve `schema_set_version`, fetch and verify `bundles/evo-catalogue-schemas-<version>.tar.gz` (+ `.sig`), extract, use as the schema baseline for plugin validation + catalogue load.
