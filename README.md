# conda-ikafssn

A standalone Conda channel for [ikafssn](https://github.com/astanabe/ikafssn).

This repository hosts only the channel index (`repodata.json`); the
`.conda` package files themselves are served from
[ikafssn's GitHub Releases](https://github.com/astanabe/ikafssn/releases)
via the [CEP-15 `base_url` mechanism](https://github.com/conda/ceps/blob/main/cep-15.md).

## Installation

Requires Conda 23.7+ or micromamba 2.0+ (for `base_url` support):

```bash
mamba install -c https://conda.ikafssn.org -c conda-forge ikafssn
```

Or with `conda`:

```bash
conda install -c https://conda.ikafssn.org -c conda-forge ikafssn
```

The `-c conda-forge` is required because ikafssn's runtime dependencies
(TBB, jsoncpp, OpenSSL, libuuid, etc.) are pulled from conda-forge.

## Supported platforms

| Subdir | Architecture | Baseline |
|---|---|---|
| `linux-64` | x86_64 Linux | glibc 2.34+ (RHEL 9 / Ubuntu 22.04 / Debian 12+) |
| `linux-aarch64` | aarch64 Linux | glibc 2.34+ |
| `osx-arm64` | Apple Silicon macOS | macOS 11+ |

## How this channel works

`mamba install -c https://conda.ikafssn.org ikafssn` triggers:

1. Mamba fetches `https://conda.ikafssn.org/<subdir>/repodata.json`
   (served from this repo via GitHub Pages with the `conda.ikafssn.org`
   custom domain).
2. The `base_url` field in `repodata.json` redirects package downloads
   to `https://github.com/astanabe/ikafssn/releases/download/v<version>/`.
3. Mamba downloads the `.conda` artifact directly from GitHub Releases,
   verifies its `sha256`, and installs.

This means the package binaries never live in this repository —
they live in ikafssn's release page.

## Release flow

When a new ikafssn release is published, the `repodata.json` files in
this repo must be regenerated. This is automated by ikafssn's
[`release.yml`](https://github.com/astanabe/ikafssn/blob/main/.github/workflows/release.yml)
workflow: after all `.conda` artifacts are uploaded to the release, a
final step regenerates the three `repodata.json` files here and pushes
them to `main`.

## License

Apache-2.0 (matching ikafssn).
