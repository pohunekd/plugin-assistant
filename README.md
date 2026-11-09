# plugin-assistant

The knowledge bundle of the built-in [Pohunek](https://github.com/zajca/pohunek)
assistant, as a context package.

| | |
|---|---|
| Repository | `pohunekd/plugin-assistant` |
| Package id | `pohunek.context.assistant` |
| Kind | context package (`[context]`, runtime feature `package-context-v1`, needs pohunek 0.41.0 or newer) |
| Content | `package/bundle/`: guides, concepts, runbooks and safety policies of `docs/knowledge/` of the matching pohunek release, the source map and the prompt contract `assistant/system.md` |

The package carries data only. `pohunek assistant` asks the daemon for the
bundle through `assistant.materialize`; the daemon serves the files of the
selected, enabled package after verifying its root against the archive digest.
Without this package a launch fails with `assistant_package_missing`.

## Release

A tag `vX.Y.Z` that equals the version in `package/runtime.toml` runs
`.github/workflows/release.yml`. It calls the reusable `plugin-release.yml` of the
pohunek core tag named in the workflow, which builds the archive with that tag's
`xtask plugin build`, attests it, and publishes `pohunek.context.assistant-X.Y.Z.tar.zst`
with `SHA256SUMS` as a release of this repository. The package is published right
after the pohunek release it belongs to, from the core tag of that release; it
needs pohunek 0.41.0 or newer.

## Build and install

```sh
scripts/assemble-bundle /path/to/pohunek-checkout
cargo xtask plugin build --dir package --output-dir <existing dir>   # in the pohunek checkout
pohunek plugin install <dir>/pohunek.context.assistant-<version>.tar.zst --sha256 <digest printed by the build> --yes
```

`package/runtime.toml` carries the package version; it follows the pohunek
release whose `docs/knowledge/` the bundle was assembled from.

## Trust

The package is installed by the digest the owner pins (`--sha256`), never
through the release catalog. The prompt contract inside it defines the safety
rules of the assistant's opening prompt, so install only archives whose digest
comes from a source you trust.
