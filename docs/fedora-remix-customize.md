---
title: Fedora Remix Customize
layout: default
nav_order: 6
---

# Fedora Remix Customize
{: .fs-8 }

Ansible playbooks for post-install customization of Fedora Remix systems.
{: .fs-5 .fw-300 }

[View on GitHub](https://github.com/tmichett/FedoraRemixCustomize){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

---

## Overview

FedoraRemixCustomize provides a set of Ansible playbooks and supporting files for configuring a Fedora Remix installation after the initial setup. It automates tasks like deploying GNOME extensions, applying desktop tweaks, and enabling hardware services such as fingerprint readers.

## Playbooks

| Playbook | Purpose |
|:---------|:--------|
| `Deploy_Gnome_Extensions.yml` | Installs and configures GNOME Shell extensions |
| `Deploy_Gnome_Tweaks.yml` | Applies GNOME desktop customizations and preferences |
| `Enable_Gnome_Extensions.yml` | Enables installed GNOME extensions for the user |
| `Enable_Fingerprint_Services.yml` | Configures and enables fingerprint authentication |

## Project Structure

| Path | Purpose |
|:-----|:--------|
| `ansible.cfg` | Ansible configuration |
| `ansible-navigator.yml` | Ansible Navigator settings |
| `inventory/` | Host inventory files |
| `Files/` | Supporting files deployed by the playbooks |
| `.ansible/` | Ansible collections and roles |

## Usage

### Running a Playbook

```bash
ansible-playbook -i inventory Deploy_Gnome_Extensions.yml
```

### With Ansible Navigator

```bash
ansible-navigator run Deploy_Gnome_Tweaks.yml
```

## Workflow

These playbooks are designed to be run after a fresh Fedora Remix installation:

1. Boot into the newly installed Fedora Remix system
2. Clone this repository
3. Run the desired playbooks to apply customizations
4. Extensions, tweaks, and services are configured automatically

This fits into the broader GitOps pipeline — the ISO provides the base system, and these playbooks handle the final-mile configuration.
