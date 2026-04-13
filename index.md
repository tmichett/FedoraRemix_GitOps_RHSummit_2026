---
title: Home
layout: default
nav_order: 1
has_children: true
---

# Fedora Remix - GitOps & Automation
{: .fs-9 }

GitHub and COPR - Red Hat Summit 2026 Presentation Materials
{: .fs-6 .fw-300 }

---

## Quickstart Guides

Get started building your own custom Fedora Remix ISO. The **Container Build is the recommended method** — it provides a consistent, reproducible environment across different systems without modifying your host.

| Guide | Description |
|:------|:------------|
| [Container Build]({% link docs/Quickstart_Container.md %}) | **Recommended.** Build a Fedora Remix ISO using the containerized build system with Podman |
| [Physical/VM Build]({% link docs/Quickstart_Physical.md %}) | Build a Fedora Remix ISO directly on a physical or virtual machine |

---

## Overview

This project documents the Fedora Remix ecosystem, a collection of tools and automation for building, customizing, and distributing a custom Fedora Linux distribution. The workflow leverages GitHub Actions, COPR, containers, and Ansible to create a fully automated GitOps pipeline.

## Projects

| Project | Description |
|:--------|:------------|
| [Remix Builder]({% link docs/remix-builder.md %}) | Containerized build environment for creating Fedora Remix ISO images using Podman |
| [Fedora Remix Lab]({% link docs/fedora-remix-lab.md %}) | KVM/libvirt scripts for creating a virtual lab environment with networked Fedora VMs |
| [Log Viewer]({% link docs/log-viewer.md %}) | Cross-platform log analysis app with ANSI color support — distributed via COPR, Flatpak, Homebrew, and Windows installer |
| [gh-md2pdf-action]({% link docs/gh-md2pdf-action.md %}) | GitHub Action to convert Markdown files to PDF with Mermaid diagrams and PDF bookmarks |
| [Fedora Remix]({% link docs/fedora-remix.md %}) | The core Fedora Remix distribution with kickstarts, themes, and build configuration |
| [Fedora Remix Customize]({% link docs/fedora-remix-customize.md %}) | Ansible playbooks for post-install customization of Fedora Remix systems |
| [Fedora Remix Tools]({% link docs/fedora-remix-tools.md %}) | PyQt5 YAML-driven menu application replacing legacy YAD/Bash scripts |

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    GitOps Pipeline                       │
├─────────────────────────────────────────────────────────┤
│                                                          │
│  Fedora Remix ──► Remix Builder ──► ISO Image            │
│       │                                                  │
│       ├── Kickstart configs                              │
│       ├── Themes & branding                              │
│       └── Package selections                             │
│                                                          │
│  Fedora Remix Lab ──► Test VMs                           │
│       └── KVM/libvirt automation                         │
│                                                          │
│  Fedora Remix Customize ──► Post-install setup           │
│       └── Ansible playbooks                              │
│                                                          │
│  Fedora Remix Tools ──► User utilities                   │
│       └── RPM via COPR                                   │
│                                                          │
│  Log Viewer ──► Diagnostics                              │
│       ├── RPM via COPR + Flatpak (Linux)                 │
│       ├── Homebrew cask (macOS)                          │
│       └── Inno Setup installer (Windows)                 │
│                                                          │
│  gh-md2pdf-action ──► PDF Documentation                  │
│       └── Markdown to PDF via GitHub Actions              │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

## Workflow

1. **Build** - The Remix Builder container automates ISO creation from kickstart configurations
2. **Test** - Fedora Remix Lab provisions VMs for validation
3. **Customize** - Ansible playbooks apply post-install configuration
4. **Package** - Tools and Log Viewer are packaged as RPMs via COPR, Flatpak bundles, Homebrew casks, and Windows installers
5. **Distribute** - ISOs, RPMs, Flatpaks, DMGs, and EXEs are published for end users across Linux, macOS, and Windows
