# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A Go-based, containerized dashboard for monitoring the health and status of a Fedora Linux development workstation. Designed to run in Docker, Podman, or Kubernetes.

## Architecture

The dashboard has two primary views:

- **Tab 1: Daily Report** — a health report composed of plug-ins, each checking a specific system concern (backups, systemd failures, DNF updates, SELinux alerts, CI/CD status, open PRs, etc.)
- **Tab 2: Live System View** — real-time CPU, memory, and other live metrics

Data sources include: `systemctl`, `dnf`, `ausearch`, Woodpecker CI API, Renovate, GitHub API, and Gitea API.

## Plug-in Model

Plug-ins are the primary extensibility mechanism. Key design principles:

- Git is the source of code; CI produces artifacts; the registry is the source of deployable artifacts; Docker/Kubernetes only run pinned digests.
- Plug-ins may ship baked into the dashboard image, as a separate service image, or as a signed OCI artifact.
- Supported plug-in sources: Local Git, Gitea, GitHub.

## Technology

- **Language:** Go (single self-contained binary)
- **Deployment:** Container (Docker/Podman/Kubernetes)
