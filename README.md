# Fedora Dashboard

A live, customizable dashboard container for monitoring the health and status of a Fedora Linux development workstation and its associated services.

## Project Goals

The dashboard presents information across two primary views (tabs or menus), with more planned for the future.

---

## Tab 1: Daily Report

A consolidated health report, powered by Plug-ins, covering the most important system and service checks. The intent is to answer the question *"Is everything OK?"* at a glance each morning. The following plug-ins ship with this project, but you can add your own by contributing a plug-in via Git. Full documentation for that process will be provided in a future update.

| Check | Description |
|---|---|
| **Backups** | Are backups completing successfully? |
| **Container & Kubernetes Updates** | Do any running containers or Kubernetes services have updates available that could be applied? |
| **Systemd Job Failures** | Are any systemd services in a failed state? |
| **Systemd Timer Failures** | Are any systemd timers failing to fire on schedule? |
| **Fedora Upgrade Available** | Is a new Fedora release available for upgrade? |
| **DNF Updates Pending** | Are there outstanding DNF package updates to apply? |
| **SELinux Alerts (24h)** | Has SELinux logged any denials or complaints in the last 24 hours? (sourced via `ausearch`) |
| **Woodpecker CI Failures** | Does Woodpecker CI report any recent build failures? |
| **Renovate PRs / Dependency Issues** | Does Renovate have any open dependency-update pull requests or unresolved issues? |
| **Open Pull Requests** | Are there any pull requests awaiting review on GitHub or Gitea? |
| **Performance Snapshot (24h)** | A summary of system performance over the last 24 hours. *(Implementation to be determined.)* |

---

## Tab 2: Live System View

A real-time view of the running system, including at minimum:

- CPU utilization
- Memory usage
- Other live metrics (to be added)

---

## Planned Additions

Additional tabs and plug-ins will be added over time.

---

## Deployment

The dashboard image can be deployed in Docker, Podman, or a Kubernetes cluster.

---

## Customization: Plug-ins

Customization is delivered through a first-class **Plug-in** model. The guiding principles for how plug-ins are built and delivered are:

- **Git is the source of code.**
- **CI is the place where code becomes an artifact.**
- **The registry is the source of deployable artifacts.**
- **Docker/Kubernetes only run pinned artifacts.**

### Planned Plug-in Workflow

```
Local Git / Gitea / GitHub
        ↓
Pull request / review
        ↓
CI build: Woodpecker, GitHub Actions, etc.
        ↓
Test + scan + build plugin
        ↓
Create one of:
  1. Dashboard image with plugins baked in
  2. Separate plugin service image
  3. Signed plugin bundle / OCI artifact
        ↓
Push to registry:
  - Gitea package / container registry
  - GitHub Container Registry (GHCR)
  - Amazon ECR
  - Harbor
  - Docker registry
        ↓
Deploy by digest:
  dashboard@sha256:...
  plugin-api@sha256:...
```

Supported plug-in sources: **Local Git**, **Gitea**, and **GitHub**.

---

## Implementation Notes

- Built with [Go](https://go.dev/) for performance and a single self-contained binary.
- Designed to run as a container for portability and easy deployment.
- Dashboard layout and checks are intended to be customizable via plug-ins.
- Data sources include: `systemctl`, `dnf`, `ausearch`, Woodpecker CI API, Renovate, GitHub API, and Gitea API.
