# AGENTS.md

## Project overview

Personal homelab infrastructure-as-code: Docker Compose stacks managed by Komodo, running on two Proxmox LXCs (a Caddy LXC and an app-stacks LXC). Not a buildable application — it's config data that Komodo applies.

## Setup

N/A — no local dependencies. Applying changes requires a running Komodo instance pointed at this repo as a Resource Sync (`komodo/resources.toml`).

## Build / Run

N/A. Komodo's "Global Auto Update" applies stack changes automatically; a specific stack can be redeployed manually from the Komodo UI.

## Test

No automated tests. Changes to `komodo/resources.toml` affect real infrastructure — review carefully and, where possible, validate against a non-production Komodo instance before merging.

## Repository structure

- `komodo/resources.toml` — Komodo server and stack resource definitions (the only file currently tracked besides docs/license)
- `README.md` documents an intended `opt/stacks/<stack-name>/compose.yaml` layout for individual stacks' Compose files, but those aren't present in this repo currently — check the actual tree rather than assuming the README's structure is fully populated.

## Commit and PR conventions

- Commit messages and PR titles must follow [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`, `ci:`, `build:`, `perf:`, `style:`, `revert:`), optionally with a scope, e.g. `fix(api): handle null response`.
- This repo squash-merges pull requests only; the PR title becomes the final commit message on `main`.
- A "Conventional Commits" CI check enforces this on both PR titles and direct-push commit messages.
- Branch protection on `main`: no force-pushes, no branch deletion, required status checks must pass.
