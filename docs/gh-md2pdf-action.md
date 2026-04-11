---
title: gh-md2pdf-action
layout: default
nav_order: 8
---

# gh-md2pdf-action
{: .fs-8 }

A GitHub Action to convert Markdown files to PDF with Mermaid diagrams, GitHub-style rendering, and automatic PDF bookmarks.
{: .fs-5 .fw-300 }

[View on GitHub](https://github.com/tmichett/gh-md2pdf-action){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[GitHub Marketplace](https://github.com/marketplace/actions/markdown-to-pdf-converter){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## Overview

`gh-md2pdf-action` is a custom GitHub Action that converts Markdown files to professionally styled PDF documents. It powers the documentation pipeline for several projects in the Fedora Remix ecosystem, including [Fedora Remix Lab]({% link docs/fedora-remix-lab.md %}).

## Features

- **Mermaid Diagram Support** -- Renders Mermaid code blocks as diagrams in PDFs
- **GitHub Markdown Styling** -- Uses GitHub's markdown CSS for consistent rendering
- **PDF Bookmarks** -- Automatically generates navigation bookmarks from headings
- **Emoji Support** -- Renders emojis using Noto Color Emoji fonts
- **Custom Pagination** -- CSS rules to prevent awkward page breaks
- **Batch Processing** -- Convert multiple files in a single step
- **Flexible Configuration** -- Use a file list or a YAML config file

## Quick Start

```yaml
- name: Convert Markdown to PDF
  uses: tmichett/gh-md2pdf-action@main
  with:
    files: README.md
    output_dir: Docs
```

## Usage Examples

### Single File

```yaml
- name: Convert Markdown to PDF
  uses: tmichett/gh-md2pdf-action@main
  with:
    files: README.md
    output_dir: Docs
```

### Multiple Files

```yaml
- name: Convert Markdown to PDF
  uses: tmichett/gh-md2pdf-action@main
  with:
    files: README.md,CHANGELOG.md,docs/guide.md
    output_dir: Docs
```

### Using a Config File

```yaml
- name: Convert Markdown to PDF
  uses: tmichett/gh-md2pdf-action@main
  with:
    config_file: config.yaml
    output_dir: Docs
```

Example `config.yaml`:

```yaml
files:
  - path: README.md
  - path: docs/user-guide.md
  - path: docs/developer-guide.md
```

### Complete Workflow with Auto-Commit

```yaml
name: Generate PDFs

on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  build-pdfs:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v5

      - name: Convert Markdown to PDF
        id: convert
        uses: tmichett/gh-md2pdf-action@main
        with:
          files: README.md,QUICKSTART.md
          output_dir: Docs

      - name: Upload PDFs as artifacts
        uses: actions/upload-artifact@v4
        with:
          name: documentation-pdfs
          path: Docs/*.pdf
          if-no-files-found: warn

      - name: Commit PDFs to repository
        if: github.event_name == 'push' && github.ref == 'refs/heads/main'
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          git add Docs/*.pdf
          if ! git diff --staged --quiet; then
            git commit -m "docs: Update PDF documentation [skip ci]"
            git push
          else
            echo "No changes to commit"
          fi
```

## How It Works

```
┌──────────────┐     ┌──────────────────────┐     ┌─────────────┐
│  Markdown    │────►│  md2pdf Container    │────►│  PDF Output │
│  Source      │     │                      │     │             │
└──────────────┘     │  1. MD → HTML        │     └─────────────┘
                     │     (GitHub styling)  │
                     │  2. Mermaid.js        │
                     │     (diagram render)  │
                     │  3. Chromium          │
                     │     (HTML → PDF)      │
                     │  4. Python            │
                     │     (PDF bookmarks)   │
                     └──────────────────────┘
```

1. **Container** -- The action runs a Docker container (`ghcr.io/tmichett/md2pdf:latest`) with all necessary tools pre-installed
2. **HTML Rendering** -- Markdown is converted to HTML with GitHub-flavored styling
3. **Diagram Processing** -- Mermaid code blocks are rendered into diagrams via Mermaid.js
4. **PDF Generation** -- Headless Chromium converts the styled HTML to PDF
5. **Bookmarks** -- Python post-processes the PDF to add navigation bookmarks from Markdown headings

## Inputs

| Input | Description | Required | Default |
|:------|:------------|:---------|:--------|
| `files` | Comma-separated list of Markdown files | No | - |
| `config_file` | Path to YAML config listing files | No | - |
| `output_dir` | Output directory for PDFs | No | `Docs` |
| `working_directory` | Working directory for file paths | No | `.` |
| `fail_on_error` | Fail if any conversion fails | No | `true` |
| `container_image` | Container image to use | No | `ghcr.io/tmichett/md2pdf:latest` |
| `build_container` | Build container if not available | No | `true` |

Either `files` or `config_file` (or both) must be provided.

## Outputs

| Output | Description |
|:-------|:------------|
| `pdfs` | Comma-separated list of generated PDF file paths |
| `pdf_count` | Number of PDFs successfully generated |

## Local Usage

The conversion container can also be used locally with Docker or Podman:

```bash
docker pull ghcr.io/tmichett/md2pdf:latest

docker run --rm \
  -v $(pwd):/workspace:rw \
  ghcr.io/tmichett/md2pdf:latest \
  /workspace/README.md \
  /workspace/README.pdf
```

## Used By

This action powers the documentation pipeline in the following projects:

- [Fedora Remix Lab]({% link docs/fedora-remix-lab.md %}) -- Converts `README.md` and `QUICKSTART.md` to PDF on every push to `main`
