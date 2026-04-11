---
title: Fedora Remix Lab
layout: default
nav_order: 6
---

# Fedora Remix Lab
{: .fs-8 }

A virtual lab environment using KVM/libvirt for testing and development.
{: .fs-5 .fw-300 }

[View on GitHub](https://github.com/tmichett/Fedora_Remix_Lab){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

---

## Overview

Fedora Remix Lab provides a collection of scripts to create and manage a Fedora-based virtual lab environment using KVM/libvirt. It creates two networked VMs, making it ideal for testing Ansible automation, clustering, or multi-node applications.

## Prerequisites

```bash
sudo dnf install qemu-kvm libvirt virt-manager libguestfs-tools virt-viewer
sudo systemctl enable --now libvirtd
```

## Quick Start

```bash
# 1. Download the base image
./download-image.sh

# 2. Create the lab environment
sudo ./create-lab-vms.sh

# 3. Start the VMs
sudo ./start-lab-vms.sh

# 4. Add host entries
sudo ./manage-hosts.sh add

# 5. Check status
sudo ./lab-status.sh

# 6. Connect to a VM
sudo virt-viewer FedoraLab1
```

## Key Components

| Script | Purpose |
|:-------|:--------|
| `create-lab-vms.sh` | Creates the two lab VMs from a base image |
| `start-lab-vms.sh` | Starts both lab VMs |
| `download-image.sh` | Downloads the base Fedora qcow2 image |
| `manage-hosts.sh` | Adds/removes VM host entries in `/etc/hosts` |
| `lab-status.sh` | Shows current status of lab VMs |
| `reset-lab.sh` | Destroys and recreates the lab from scratch |
| `hosts.local` | Local hosts file template |
| `inventory/` | Ansible inventory for the lab VMs |

## Lab Architecture

The lab creates two VMs on an isolated virtual network:

```
┌──────────────────────────────────────┐
│           Host Machine               │
│                                      │
│  ┌──────────────┐ ┌──────────────┐   │
│  │  FedoraLab1  │ │  FedoraLab2  │   │
│  │              │ │              │   │
│  └──────┬───────┘ └──────┬───────┘   │
│         │                │           │
│     ┌───┴────────────────┴───┐       │
│     │   Virtual Network      │       │
│     └────────────────────────┘       │
└──────────────────────────────────────┘
```

## Documentation Build Process

The Fedora Remix Lab repository uses a GitHub Actions workflow to automatically generate PDF documentation from its Markdown source files. This is powered by the custom [gh-md2pdf-action](https://github.com/tmichett/gh-md2pdf-action), a GitHub Action that converts Markdown to PDF with support for Mermaid diagrams, GitHub-style rendering, and automatic PDF bookmarks.

### How It Works

```
┌──────────────┐     ┌──────────────────┐     ┌──────────────┐
│  Push to     │────►│  GitHub Actions  │────►│  Docs/*.pdf  │
│  main branch │     │  workflow        │     │  committed   │
└──────────────┘     └────────┬─────────┘     └──────────────┘
                              │
                     ┌────────▼─────────┐
                     │ gh-md2pdf-action  │
                     │                  │
                     │ 1. Pull md2pdf   │
                     │    container     │
                     │ 2. Convert MD    │
                     │    to HTML       │
                     │ 3. Render with   │
                     │    Chromium      │
                     │ 4. Generate PDF  │
                     │    bookmarks     │
                     └──────────────────┘
```

1. **Trigger** -- The workflow runs on every push to `main` or via manual `workflow_dispatch`
2. **Conversion** -- The `gh-md2pdf-action` processes each Markdown file (`README.md`, `QUICKSTART.md`) through a containerized pipeline:
   - Markdown is rendered to HTML with GitHub-flavored styling
   - Mermaid diagram blocks are rendered via Mermaid.js
   - Headless Chromium generates the PDF
   - PDF bookmarks are created from Markdown headings using Python
3. **Artifacts** -- The generated PDFs are uploaded as downloadable build artifacts
4. **Auto-commit** -- On pushes to `main`, the workflow commits the updated PDFs back to the `Docs/` directory with a `[skip ci]` tag to prevent a workflow loop

### Workflow Configuration

The workflow at `.github/workflows/main.yml` uses the action like this:

```yaml
- name: Convert Markdown to PDF
  uses: tmichett/gh-md2pdf-action@main
  with:
    files: README.md,QUICKSTART.md
    output_dir: Docs
```

### Source Files and Output

| Source | Output |
|:-------|:-------|
| `README.md` | `Docs/README.pdf` |
| `QUICKSTART.md` | `Docs/QUICKSTART.pdf` |

The `gh-md2pdf-action` can also be configured via a YAML config file for more complex projects, and supports custom container images, working directories, and error handling options. See the [gh-md2pdf-action documentation](https://github.com/tmichett/gh-md2pdf-action) for full details.
