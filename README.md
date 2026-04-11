# Fedora Remix GitOps - Red Hat Summit 2026

Documentation site for the Fedora Remix ecosystem, covering the tools and automation used to build, customize, and distribute a custom Fedora Linux distribution.

**Live Site:** [https://tmichett.github.io/FedoraRemix_GitOps_RHSummit_2026/](https://tmichett.github.io/FedoraRemix_GitOps_RHSummit_2026/)

## Projects Documented

| Project | Repository | Description |
|:--------|:-----------|:------------|
| Remix Builder | [RemixBuilder](https://github.com/tmichett/RemixBuilder) | Containerized build environment for Fedora Remix ISOs |
| Fedora Remix Lab | [Fedora_Remix_Lab](https://github.com/tmichett/Fedora_Remix_Lab) | KVM/libvirt virtual lab automation |
| Log Viewer | [log_viewer](https://github.com/tmichett/log_viewer) / [log_viewer_copr](https://github.com/tmichett/log_viewer_copr) | PyQt6 log analysis application |
| gh-md2pdf-action | [gh-md2pdf-action](https://github.com/tmichett/gh-md2pdf-action) | GitHub Action for Markdown to PDF conversion |
| Fedora Remix | [Fedora_Remix](https://github.com/tmichett/Fedora_Remix) | Core distribution kickstarts and configuration |
| Fedora Remix Customize | [FedoraRemixCustomize](https://github.com/tmichett/FedoraRemixCustomize) | Post-install Ansible playbooks |
| Fedora Remix Tools | [Fedora_Remix_Tools](https://github.com/tmichett/Fedora_Remix_Tools) | PyQt5 menu application and RPM packaging |

## Local Development

```bash
bundle install
bundle exec jekyll serve
```

Then open [http://localhost:4000/FedoraRemix_GitOps_RHSummit_2026/](http://localhost:4000/FedoraRemix_GitOps_RHSummit_2026/).

## Deployment

The site is automatically built and deployed to GitHub Pages on every push to `main` via the GitHub Actions workflow in `.github/workflows/pages.yml`.
