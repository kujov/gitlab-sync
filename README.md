<div align="center">
  <img src="assets/gitlab-sync.png" width="40%" alt="GitLab Sync">
</div>

# GitLab Sync - GitHub Action

**GitLab Sync** is a GitHub Action designed for straightforward synchronization of code from a GitHub repository to a GitLab repository.

## Motivation

This GitHub Action was developed to provide a reliable and easy-to-use solution for syncing repositories between GitHub and GitLab, addressing common challenges found in existing solutions.

## How to Use

Follow these steps to integrate this GitHub Action into your workflow:

### Step 1: Configure GitHub Secrets

Before using the action, you need to add the following secrets to your GitHub repository's settings:

- `GITLAB_URL`: The HTTPS URL of your GitLab repository (e.g., `https://gitlab.com/yourusername/yourrepo.git`).
- `USERNAME`: Your GitLab username.
- `GITLAB_PAT`: Your GitLab Personal Access Token with appropriate permissions to push to the repository.

### Step 2: Add the Workflow to Your GitHub Repository

Create a workflow file (e.g., `.github/workflows/sync-to-gitlab.yml`) in your GitHub repository with the following configuration:

```yaml
name: Sync Repo to GitLab

on:
  push

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
    - name: Sync to GitLab
      uses: keninkujovic/gitlab-sync@2.0.0
      with:
        gitlab_url: ${{ secrets.GITLAB_URL }}
        username: ${{ secrets.USERNAME }}
        gitlab_pat: ${{ secrets.GITLAB_PAT }}
```

**Note:** It's recommended to specify which branches trigger the sync (e.g., `main`, `master`) and to include `actions/checkout@v3` (or a newer version) to ensure the action has access to your repository's content. Using `fetch-depth: 0` ensures all history is fetched, which is often necessary for a clean sync.

### Step 3: Push Changes to GitHub

Once the workflow is set up, any push to the configured branches in your GitHub repository will automatically trigger the synchronization process to your GitLab repository.

## Inputs

The following inputs are required for this action:

- `gitlab_url`: (Required) The HTTPS URL of your GitLab repository.
- `username`: (Required) Your GitLab username.
- `gitlab_pat`: (Required) Your GitLab Personal Access Token. Ensure this token has sufficient permissions to write to the target GitLab repository.

## Feedback and Contributions

Feedback and contributions are highly valued. Please feel free to open an issue for bug reports or feature requests, or submit a pull request with improvements.

If you find this action useful, consider starring the repository!

## Disclaimer

This action was developed to fulfill a specific need. While it has been tested, users should verify its functionality in their own environments. Please use it at your own discretion.
