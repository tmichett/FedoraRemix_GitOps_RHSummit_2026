---
title: Fedora Remix Tools
layout: default
nav_order: 7
---

# Fedora Remix Tools
{: .fs-8 }

PyQt5 YAML-driven menu application and RPM packaging for Fedora Remix utilities.
{: .fs-5 .fw-300 }

[View on GitHub](https://github.com/tmichett/Fedora_Remix_Tools){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

---

## Overview

Fedora Remix Tools is the successor to the legacy YAD/Bash scripts for launching Remix utilities. It replaces the old approach with a PyQt5 Python application driven entirely by a `config.yml` file. The menu structure, commands, icons, and labels are all defined in YAML, making it easy to customize without modifying code.

## Configuration

The menu is defined in `config.yml`:

```yaml
icon: smallicon.png
logo: logo.png
menu_title: Title of Menu
menu_items:
  - name: Separator 1
    items:
      - name: Menu Item 1
        command: ls -alF | grep travis
      - name: Menu Item 2
        command: echo "This is a test"
  - name: Separator 2
    items:
      - name: Menu Item 1
        command: echo "This is another test"
```

## Running from Source

```bash
uv venv menu_venv --python=3.12
source menu_venv/bin/activate
uv pip install PyQt5 PyYaml
python menu.py
```

## RPM Packaging

The project includes full RPM build scaffolding for distribution through Fedora COPR:

| Path | Purpose |
|:-----|:--------|
| `rpmbuild/` | RPM spec files and sources |
| `RPM_Build.sh` | Build the RPM package |
| `RPM_Build_Clean.sh` | Clean build with fresh spec |
| `RPM_All.sh` | Full build pipeline |
| `create_tarball.sh` | Create source tarball for RPM |
| `.copr/` | COPR build configuration |
| `.github/` | CI/CD workflows |

### Building an RPM

```bash
mkdir -p ~/rpmbuild/{SPECS,SOURCES}
cp config.yml menu.py logo.png smallicon.png RHCI.desktop ~/rpmbuild/SOURCES
rpmbuild -ba ~/rpmbuild/SPECS/rhci_foundation.spec
```

## COPR Distribution

The RPM is published to Fedora COPR for easy installation on Fedora systems. The `.copr/` directory contains the build configuration, and GitHub Actions workflows automate the build-and-publish cycle.
