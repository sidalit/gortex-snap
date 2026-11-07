<div align="center">

<img src="snap/gui/gortex.png" alt="Gortex logo" width="128" />

# Gortex

**Search code, explore repository graphs and give AI agents precise context.**

[![gortex](https://snapcraft.io/gortex/badge.svg)](https://snapcraft.io/gortex)
[![Base: core26](https://img.shields.io/badge/Base-core26-2C8C3C?logo=canonical&logoColor=white)](https://snapcraft.io/core26)
![Confinement: strict](https://img.shields.io/badge/Confinement-strict-2C8C3C)
[![Maintainer: sidaliteir](https://img.shields.io/badge/Maintainer-sidaliteir-2C8C3C?logo=snapcraft&logoColor=white)](https://snapcraft.io/publisher/sidaliteir)
![Architectures: amd64, arm64](https://img.shields.io/badge/Architectures-amd64%20%7C%20arm64-2C8C3C?logo=linux&logoColor=white)

[Getting Started](#getting-started) · [Configure](#configure) · [Limitations](#limitations) · [Build](#build) · [Support](#support) · [License](#license)

Unofficial snap packaging for [Gortex](https://github.com/zzet/gortex), a code intelligence engine with a CLI, repository graphs and an MCP server.

</div>

---

## Getting Started

<a href="https://snapcraft.io/gortex"><img alt="Get it from the Snap Store" src="https://snapcraft.io/en/dark/install.svg" width="180"></a>

```bash
sudo snap install gortex
gortex track ~/Projects/my-app
gortex daemon start --detach
gortex daemon status
```

Configure your coding agent's MCP client to run `/snap/bin/gortex` with `mcp` as its argument. Set the client's working directory to the project where supported. The CLI and MCP client share the same per-user daemon and index.

## Configure

Use `.gortex.yaml` in a project or `~/snap/gortex/common/config/gortex/config.yaml` for global settings. Indexes and downloaded models live under `~/snap/gortex/common/data/gortex`; cache, logs and the private daemon socket also stay under `~/snap/gortex/common`. There are no `snap set` options.

The daemon normally uses a private Unix socket. To enable the HTTP API and Streamable HTTP MCP endpoint locally:

```bash
gortex daemon stop
gortex daemon start --detach --http-addr 127.0.0.1:7411
```

The endpoints are `http://127.0.0.1:7411/v1/health` and `http://127.0.0.1:7411/mcp`. Upstream requires a token for non-loopback listeners. The [separate web UI](https://github.com/gortexhq/web) can connect to this API; explicitly allow its trusted origin using `--http-allowed-origin` and `--cors-origin` when starting the daemon.

Built-in GloVe embeddings work offline. Optional `embedding.provider: local` downloads the pure-Go MiniLM model; `api` uses an Ollama or compatible embedding endpoint. HTTP LLM providers are available through [upstream model configuration](https://github.com/zzet/gortex/blob/main/docs/llm.md).

For repositories on external storage:

```bash
sudo snap connect gortex:removable-media
```

## Limitations

- Projects must be under accessible home directories or connected external storage. Host SDKs, language servers and CLI-based AI providers are unavailable; bundled parsers, Git analysis, pure-Go local embeddings and network model providers remain available.
- Native llama.cpp, ONNX Runtime and XLA backends are not included. Use Ollama or another HTTP provider for local generative models. The web UI is a separate application and is not bundled.
- Automatic installation into host agents, host service installation, system-wide uninstall and self-upgrade cannot manage the host from confinement. Register MCP manually, manage the daemon with `gortex daemon start/stop`, and update with `sudo snap refresh gortex`.
- Start the daemon again after login or snap refresh if needed. Shared indexes and configuration survive refreshes; snap revert does not restore an earlier index schema.

## Build

```bash
git clone https://github.com/sidalit/gortex-snap.git
cd gortex-snap
sudo snap install snapcraft --classic
snapcraft pack
```

## Support

- **Installation or snap issues:** [open a packaging issue](https://github.com/sidalit/gortex-snap/issues).
- **Application bugs:** [report them upstream](https://github.com/zzet/gortex/issues).

## License

Snap packaging: [MIT](LICENSE).
