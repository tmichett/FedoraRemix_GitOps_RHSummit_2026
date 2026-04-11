---
title: Remix Builder
layout: default
nav_order: 2
---

# Remix Builder
{: .fs-8 }

A containerized build environment for creating Fedora Remix ISO images.
{: .fs-5 .fw-300 }

[View on GitHub](https://github.com/tmichett/RemixBuilder){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

---

## Overview

Remix Builder provides a Podman-based container that automates the Fedora Remix build process. It encapsulates all the tooling needed to produce bootable ISO images from kickstart configurations, eliminating the need to install build dependencies directly on the host system.

## Prerequisites

- **Podman** installed and configured
- **GitHub Personal Access Token** with `write:packages` permission (for pushing to GHCR)
- **SSH key** for GitHub access (used by the container for git operations)
- **Fedora Remix project** directory on the local system

## Configuration

The build is driven by `config.yml`:

```yaml
Container_Properties:
  Fedora_Version: "43"
  SSH_Key_LLocation: "~/.ssh/github_id"
  Fedora_Remix_Location: "/path/to/your/output/directory"
  GitHub_Registry_Owner: "your-github-username"
```

Key points:
- `Fedora_Version` must match the version in the Fedora Remix project's `/Setup/config.yml`
- `Image_Name` is automatically generated from `GitHub_Registry_Owner` and `Fedora_Version`
- Format: `ghcr.io/{GitHub_Registry_Owner}/fedora-remix-builder:{Fedora_Version}`

## Key Components

| File | Purpose |
|:-----|:--------|
| `Containerfile` | Defines the build container image |
| `Build_Remix.sh` | Main build script (copy to Fedora Remix project root) |
| `config.yml` | Build configuration |
| `build.sh` | Container image build script |
| `push.sh` | Push container image to GHCR |
| `entrypoint.sh` | Container entrypoint script |
| `exit-container.sh` | Cleanup on container exit |
| `Container_Reqs/` | Container dependency files |

## Usage

### Building the Container Image

```bash
./build.sh
```

### Pushing to GitHub Container Registry

```bash
./push.sh
```

### Building a Fedora Remix ISO

```bash
./Build_Remix.sh
```

## Container Workflow

1. The container pulls the Fedora Remix project via SSH/git
2. Kickstart files define the package set and configuration
3. `livemedia-creator` or equivalent tooling builds the ISO inside the container
4. The finished ISO is written to the configured output directory on the host
