---
title: Log Viewer
layout: default
nav_order: 7
---

# Log Viewer
{: .fs-8 }

A PyQt6 GUI application for viewing, searching, and analyzing log files.
{: .fs-5 .fw-300 }

[Application Repository](https://github.com/tmichett/log_viewer){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[COPR Package](https://github.com/tmichett/log_viewer_copr){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## Overview

Log Viewer is a cross-platform GUI application for viewing, searching, and analyzing log files. It consists of two repositories:

- **log_viewer** — The main application source, CI/CD workflows, Flatpak packaging, and documentation
- **log_viewer_copr** — COPR/RPM packaging with the spec file, desktop entry, and bundled sources for distribution

## Features

### Core Functionality
- Multi-format support for `.log`, `.out`, `.txt`, and other text files
- Large file handling with efficient memory management
- ANSI color code parsing and display
- Asynchronous, non-blocking file loading with progress feedback

### Search & Navigation
- Real-time search with debounced input
- Bidirectional find (Next/Previous)
- Entire-line highlighting for search matches
- Search result caching for efficient navigation

### Text Highlighting
- Configurable highlight terms via GUI
- Custom color picker per term
- YAML-based configuration files
- Multiple config profiles for different log types

### User Interface
- Dark theme with custom styling
- Adjustable font size (6–72pt)
- Responsive layout
- Monospace font for structured log output

### Performance
- Chunked file loading (256KB blocks)
- Background threading via QThreadPool
- Document block limits to control memory usage

## Installation

### Prerequisites

- Python 3.8+
- Linux, macOS, or Windows

### From COPR (Fedora)

The RPM package is available through Fedora COPR, providing system-level integration with a desktop entry and launcher script.

### Homebrew (macOS)

Log Viewer is available as a Homebrew cask with native support for both Apple Silicon and Intel Macs.

```bash
brew tap tmichett/logviewer
brew install --cask logviewer
```

To upgrade to a new version:

```bash
brew update
brew upgrade --cask logviewer
```

To uninstall:

```bash
brew uninstall --cask logviewer
```

### From Source

```bash
pip install PyQt6 PyYAML
python log_viewer.py
```

### Flatpak

A Flatpak build script and metadata are included in the main repository for sandboxed distribution.

## Repository Structure

### log_viewer (Application)

| Path | Purpose |
|:-----|:--------|
| `log_viewer.py` | Main application entry point |
| `rpmbuild/` | RPM build specs and scripts |
| `com.michettetech.LogViewer.*` | Flatpak metadata and manifest |
| `.github/workflows/` | CI/CD pipelines |
| `RPM_*.sh` | RPM build helper scripts |

### log_viewer_copr (Packaging)

| Path | Purpose |
|:-----|:--------|
| `LogViewer.spec` | RPM spec file for COPR builds |
| `LogViewer.desktop` | Desktop entry file |
| `log_viewer_start.sh` | Launcher wrapper script |
| `log_viewer/` | Bundled application sources |
| `config.yml` | Default configuration |

## CI/CD Pipeline

Log Viewer has a comprehensive GitHub Actions pipeline that builds, packages, and releases the application across all supported platforms. Each platform build workflow produces artifacts independently, and the release workflows aggregate them into a single GitHub release.

### Pipeline Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    Push tag (v*.*.*)                             │
│                         │                                       │
│    ┌────────────────────┼────────────────────┐                  │
│    │                    │                    │                  │
│    ▼                    ▼                    ▼                  │
│ ┌────────┐       ┌───────────┐       ┌───────────┐             │
│ │ macOS  │       │  Windows  │       │   Linux   │             │
│ │ Dual   │       │   Build   │       │  RPM +    │             │
│ │ Arch   │       │           │       │  Flatpak  │             │
│ └───┬────┘       └─────┬─────┘       └─────┬─────┘             │
│     │                  │                   │                   │
│     ▼                  ▼                   ▼                   │
│  arm64 DMG         Setup.exe           .rpm  .flatpak          │
│  x86_64 DMG        Portable .exe       .src.rpm               │
│     │                  │                   │                   │
│     └──────────────────┼───────────────────┘                   │
│                        ▼                                       │
│              ┌──────────────────┐                               │
│              │  Release on Tag  │                               │
│              │  (draft release) │                               │
│              └────────┬─────────┘                               │
│                       │                                        │
│                       ▼                                        │
│              ┌──────────────────┐                               │
│              │    Publish to    │                               │
│              │    Homebrew Tap  │                               │
│              └──────────────────┘                               │
└─────────────────────────────────────────────────────────────────┘
```

### Platform Build Workflows

#### macOS Dual Architecture (`macos_build_dual.yml`)

Builds native app bundles and DMG installers for both Apple Silicon and Intel Macs.

- **Triggers:** Push to `main`, version tags (`v*`), pull requests, manual dispatch
- **Runners:** `macos-latest` (arm64), `macos-15-intel` (x86_64)
- **Process:**
  1. Builds a PyInstaller executable for the target architecture
  2. Creates a macOS `.app` bundle
  3. Packages into a DMG using `create-dmg`
  4. Tests both the app bundle and DMG mount/unmount

| Artifact | Description |
|:---------|:------------|
| `LogViewer-{version}-macOS-arm64.dmg` | Apple Silicon (M1/M2/M3/M4) |
| `LogViewer-{version}-macOS-x86_64.dmg` | Intel Macs |

#### Windows Build (`windows_build.yml`)

Builds a standalone executable and an installer on Windows.

- **Triggers:** Push to `main`, version tags, pull requests, manual dispatch
- **Runner:** `windows-latest`
- **Process:**
  1. Builds a PyInstaller executable with version info
  2. Converts the PNG icon to ICO format
  3. Creates a Windows installer using Inno Setup
  4. Tests the executable

| Artifact | Description |
|:---------|:------------|
| `LogViewer-{version}-Setup.exe` | Windows installer (Inno Setup) |
| `LogViewer-{version}.exe` | Portable executable |

#### RPM Build (`rpm_build.yml`)

Builds RPM and Source RPM packages inside a Fedora container.

- **Triggers:** Push to `main`, version tags, pull requests, manual dispatch
- **Runner:** `ubuntu-latest` with `fedora:latest` container
- **Process:**
  1. Installs RPM build tools and `uv` for Python dependency management
  2. Updates the spec file version from `Build_Version`
  3. Creates a virtual environment and builds a PyInstaller executable
  4. Runs `rpmbuild` to produce binary and source RPMs

| Artifact | Description |
|:---------|:------------|
| `LogViewer-{version}-0.x86_64.rpm` | Binary RPM for Fedora/RHEL |
| `LogViewer-{version}-0.src.rpm` | Source RPM for rebuilding |

#### Flatpak Build (`flatpak_build.yml`)

Builds a Flatpak bundle for universal Linux distribution.

- **Triggers:** Push to `main`, version tags, pull requests, manual dispatch
- **Runner:** `ubuntu-latest`
- **Process:**
  1. Builds a PyInstaller executable from source
  2. Installs the Freedesktop 23.08 runtime and SDK from Flathub
  3. Runs `flatpak-builder` with the app manifest (`com.michettetech.LogViewer.yaml`)
  4. Creates a distributable `.flatpak` bundle

| Artifact | Description |
|:---------|:------------|
| `LogViewer-{version}.flatpak` | Flatpak bundle |

### Release Workflows

#### Release on Tag (`release_on_tag.yml`)

The primary release workflow, triggered automatically when a version tag is pushed.

- **Trigger:** Push of a tag matching `v*.*.*`
- **Process:**
  1. Waits 5 minutes for all platform builds to complete
  2. Downloads artifacts from the latest successful run of each build workflow
  3. Organizes all platform artifacts (DMGs, EXEs, RPMs, Flatpak)
  4. Creates a **draft** GitHub release with all artifacts attached and platform-specific installation instructions

#### Automated Comprehensive Release (`automated_comprehensive_release.yml`)

An event-driven alternative that listens for build workflow completions.

- **Trigger:** `workflow_run` completion from any of the four platform build workflows
- **Process:** Validates the triggering tag, waits for all builds, downloads artifacts, and creates a draft release
- Only runs when the triggering workflow was for a valid version tag

#### Manual Comprehensive Release (`manual_comprehensive_release.yml`)

A fallback workflow for creating releases when automation doesn't trigger correctly.

- **Trigger:** Manual `workflow_dispatch` with a tag name input
- **Process:** Validates the tag format, creates a draft release with installation instructions, and provides steps for manually attaching artifacts from completed builds

### Homebrew Publishing (`publish_homebrew.yml`)

Publishes macOS builds to a Homebrew tap after a release is published.

- **Trigger:** GitHub release published, or manual `workflow_dispatch` with a tag name
- **Process:**
  1. Downloads the arm64 and x86_64 DMGs from the release assets
  2. Calculates SHA256 checksums for both architectures
  3. Generates a Homebrew cask (`logviewer.rb`) with `Hardware::CPU.intel?` architecture detection
  4. Commits and pushes the cask to the [homebrew-logviewer](https://github.com/tmichett/homebrew-logviewer) tap repository

| Secret | Purpose |
|:-------|:--------|
| `HOMEBREW_TAP_TOKEN` | GitHub PAT with write access to the tap repository |

### Version Management

All workflows read the version from `rpmbuild/SOURCES/Build_Version`:

```
VERSION=3.2.0
```

This single file drives version numbers across all platform builds, release notes, and artifact naming.
