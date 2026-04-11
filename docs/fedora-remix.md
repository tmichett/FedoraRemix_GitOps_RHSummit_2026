---
title: Fedora Remix
layout: default
nav_order: 2
---

# Fedora Remix
{: .fs-8 }

The core Fedora Remix distribution — kickstarts, themes, and build configuration.
{: .fs-5 .fw-300 }

[View on GitHub](https://github.com/tmichett/Fedora_Remix){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

---

## Overview

Fedora Remix is a customized Fedora Linux live ISO with pre-configured packages, themes, and utilities. This repository contains everything needed to define and build the distribution: kickstart files, GNOME/COSMIC themes, branding assets, setup scripts, and integrated documentation in AsciiDoc format.

## Quick Start

### Prerequisites

- Podman installed
- Fedora Remix Builder container (from [RemixBuilder]({% link docs/remix-builder.md %}))
- At least 10GB free disk space
- SSH key for GitHub access (optional)

### Building the ISO

```bash
# Configure the build
vim config.yml

# Interactive mode — choose from available kickstarts
./Build_Remix.sh

# Or specify directly
./Build_Remix.sh -k FedoraRemix        # GNOME desktop
./Build_Remix.sh -k FedoraRemixCosmic  # COSMIC desktop

# List available kickstarts
./Build_Remix.sh -l
```

### Output

- Location: `{Fedora_Remix_Location}/FedoraRemix/{KickstartName}.iso`
- Size: ~7–8 GB
- Build time: ~30 minutes

## Key Components

| Path | Purpose |
|:-----|:--------|
| `FedoraRemix*.ks` | Kickstart files defining package sets and configuration |
| `Setup/` | Setup scripts and configuration |
| `config.yml` | Build configuration |
| `Grub_Theme/` | Custom GRUB bootloader theme |
| `themes/` | Desktop themes and icons |
| `fonts/` | Custom font packages |
| `WiFi/` | WiFi configuration utilities |
| `Tools/` | Helper tools included in the ISO |
| `Files/` | Additional files bundled into the image |
| `WebFiles/` | Web-related assets |
| `YAD/` | Legacy YAD-based utilities |
| `Chapters/` / `docs/` | AsciiDoc documentation |

## Available Kickstarts

| Kickstart | Desktop |
|:----------|:--------|
| `FedoraRemix` | GNOME (default) |
| `FedoraRemixCosmic` | COSMIC desktop |

## Integration

The Fedora Remix project integrates with other components in the ecosystem:

- **[Remix Builder]({% link docs/remix-builder.md %})** — Container that executes the ISO build
- **[Fedora Remix Customize]({% link docs/fedora-remix-customize.md %})** — Post-install Ansible automation
- **[Fedora Remix Tools]({% link docs/fedora-remix-tools.md %})** — User-facing utilities included in the ISO
