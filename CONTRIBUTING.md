# Contributing to docker

This repo is Zack's personal homelab infrastructure: Docker Compose stacks run via [Komodo](https://komo.do) across two Proxmox LXCs, defined by `komodo/resources.toml`. It's public for reference, but it describes one specific homelab setup rather than a general-purpose tool — changes are mostly personal infra tweaks.

## Getting started

There's no local build — this repo is data (Komodo resource definitions), applied by pointing a Komodo instance at it as a Resource Sync.

## Development

Edits are typically to `komodo/resources.toml` (server/stack definitions). Verify a change by running the Komodo sync against a non-production instance if possible before merging, since this drives real infrastructure.

## Commit messages and pull requests

This repo uses [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, `docs:`, `chore:`, etc.). Pull requests are squash-merged, and the **PR title** becomes the commit on `main` — so PR titles must follow this format. This is enforced automatically by the "Conventional Commits" check.

Direct pushes to `main` are allowed but must also use a Conventional Commits-formatted commit message (validated by the same check).

## Opening a pull request

1. Create a branch off `main`.
2. Make your changes.
3. Open a pull request with a Conventional Commits-formatted title.
4. Wait for CI to pass — required checks must be green before merge.

## Reporting issues

Use [GitHub Issues](../../issues).
