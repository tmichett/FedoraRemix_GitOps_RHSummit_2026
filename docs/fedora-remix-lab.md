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

## Documentation

Additional documentation is available in the `Docs/` directory of the repository, including detailed setup instructions and the Quick Start Guide (`QUICKSTART.md`) for Fedora Remix Lab ISO users.
