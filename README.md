<div align="center">
  <img src="assets/gitlab-sync.png" width="35%" alt="GitLab Sync">
  <h1>GitLab Sync</h1>
  <p>A GitHub Action for automatically mirroring your GitHub repository to GitLab.</p>

  [![GitHub Marketplace](https://img.shields.io/badge/Marketplace-GitLab%20Sync-blue?logo=github)](https://github.com/marketplace/actions/sync-to-gitlab)
  [![Latest Release](https://img.shields.io/github/v/release/kujov/gitlab-sync)](https://github.com/kujov/gitlab-sync/releases)
  [![License](https://img.shields.io/badge/license-MIT-blue)](LICENSE)
</div>

---

## Overview

GitLab Sync pushes your repository to GitLab on every push. It handles the full checkout, validation, and push — including full git history and optional Git LFS objects.

**Features:**
- Full history sync (`fetch-depth: 0`)
- Optional Git LFS support
- Optional force push
- Clear error messages for misconfigured secrets

---

## Quick Start

### 1. Add Secrets

In your GitHub repository go to `Settings > Secrets and variables > Actions` and add:

| Secret | Description |
|--------|-------------|
| `GITLAB_URL` | Full HTTPS URL of your GitLab repo — e.g. `https://gitlab.com/user/repo.git` |
| `USERNAME` | Your GitLab username |
| `GITLAB_PAT` | A GitLab Personal Access Token with `write_repository` scope |

> **Tip:** Generate a GitLab PAT at `User Settings > Access Tokens`. Minimum required scope is `write_repository`.

### 2. Add a Workflow

Create `.github/workflows/sync-to-gitlab.yml` in your repository:

```yaml
name: Sync to GitLab

on:
  push:
    branches: [main]

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - name: Sync to GitLab
        uses: kujov/gitlab-sync@main
        with:
          gitlab_url: ${{ secrets.GITLAB_URL }}
          username: ${{ secrets.USERNAME }}
          gitlab_pat: ${{ secrets.GITLAB_PAT }}
```

That's it, every push to `main` will mirror to your GitLab repository.

---

## Configuration

### Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `gitlab_url` | HTTPS URL of the target GitLab repository | Yes | — |
| `username` | Your GitLab username | Yes | — |
| `gitlab_pat` | GitLab PAT with `write_repository` scope | Yes | — |
| `force_push` | Force push to GitLab, overwriting remote history | No | `false` |
| `git_lfs` | Also push Git LFS objects to GitLab | No | `false` |

### Sync all branches

Change the trigger to sync on pushes to any branch:

```yaml
on:
  push
```

### Force push

Use with caution — this overwrites the GitLab repository history:

```yaml
      - uses: kujov/gitlab-sync@main
        with:
          gitlab_url: ${{ secrets.GITLAB_URL }}
          username: ${{ secrets.USERNAME }}
          gitlab_pat: ${{ secrets.GITLAB_PAT }}
          force_push: true
```

### Git LFS

To also sync LFS objects, enable `git_lfs`. Make sure LFS is enabled on your GitLab repository before using this.

```yaml
      - uses: kujov/gitlab-sync@main
        with:
          gitlab_url: ${{ secrets.GITLAB_URL }}
          username: ${{ secrets.USERNAME }}
          gitlab_pat: ${{ secrets.GITLAB_PAT }}
          git_lfs: true
```

---

## Troubleshooting

**Authentication failed**
Verify `GITLAB_PAT` is valid and has `write_repository` scope. Tokens expire — check `User Settings > Access Tokens` on GitLab.

**Push rejected (non-fast-forward)**
The GitLab repository has commits not in GitHub. Use `force_push: true` to overwrite, or manually reconcile histories.

**LFS objects not syncing**
Ensure Git LFS is enabled on the GitLab repository (`Settings > General > Visibility > Git LFS`) and that `git_lfs: true` is set.

**Invalid `gitlab_url` format**
The URL must be a full HTTPS URL ending in `.git`, e.g. `https://gitlab.com/user/repo.git`.

---

## Contributing

Issues and pull requests are welcome. If this action is useful to you, consider starring the repository.
