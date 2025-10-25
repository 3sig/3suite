# 3suite Meta Repository

This is a meta repository for managing all 3suite components using [mani](https://github.com/alajmo/mani).

## Getting Started

### Initialize all repositories
```bash
mani sync
```
This will clone all repositories defined in `mani.yaml` to their respective directories.

### Update all repositories
```bash
mani exec --all git pull
```
This will pull the latest changes from the remote repositories for all components. This syntax is also useful for running any git command across all repositories, like `git fetch`, `git checkout`, etc.

### Check status of all repositories
```bash
mani run --all status
```
This will run `git status` across all repositories to see their current state. See `mani.yaml` for implementation.
