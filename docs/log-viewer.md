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
