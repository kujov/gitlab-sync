<div align="center">
  <img src="assets/gitlab-sync.png" width="40%" alt="GitLab Sync">
</div>

<div align="center">
  <h1>GitLab Sync - GitHub Action</h1>

  <p><strong>GitLab Sync</strong> is a GitHub Action designed for seamless and automated synchronization of code from a GitHub repository to a GitLab repository.</p>
</div>

## Motivation

This GitHub Action provides a free, simple, and reliable method for mirroring repositories from GitHub to GitLab. It's designed to be straightforward to implement and operate effectively.

## How to Use

Integrate GitLab Sync into your workflow by following these steps:

### 1. Configure GitHub Secrets

Before using the action, you must add the following secrets to your GitHub repository (`Settings > Secrets and variables > Actions`):

-   `GITLAB_URL`: The HTTPS URL of your target GitLab repository (e.g., `https://gitlab.com/yourusername/yourrepo.git`).
-   `USERNAME`: Your GitLab username.
-   `GITLAB_PAT`: Your GitLab Personal Access Token. Ensure this token has `write_repository` scope to push to the repository.

### 2. Create a Workflow File

Add a workflow file to your GitHub repository (e.g., `.github/workflows/sync-to-gitlab.yml`):

```yaml
name: Sync Repository to GitLab

on:
  push

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
    - name: Sync to GitLab
      uses: keninkujovic/gitlab-sync@2.1.0
      with:
        gitlab_url: ${{ secrets.GITLAB_URL }}
        username: ${{ secrets.USERNAME }}
        gitlab_pat: ${{ secrets.GITLAB_PAT }}
```

**Important Considerations:**

-   **Branch Specificity**: It's highly recommended to specify which branches trigger the sync (e.g., `main`, `master`) in the `on: push: branches:` section of your workflow to avoid unintended syncs.
-   **`actions/checkout`**: Always include `actions/checkout@vX` (preferably the latest version) before this action to ensure your repository's code is available.
-   **`fetch-depth: 0`**: Using `fetch-depth: 0` with `actions/checkout` is crucial for ensuring the entire Git history is fetched. This is often necessary for a clean and successful synchronization, especially if you need to sync tags or ensure branch histories are complete.

### 3. Push Changes to GitHub

Once the workflow is configured, any push to the specified branches in your GitHub repository will automatically trigger the synchronization process to your GitLab repository.

## Action Inputs

The action supports the following input parameters:

| Parameter    | Description                                                                                                | Required | Default |
|--------------|------------------------------------------------------------------------------------------------------------|----------|---------|
| `gitlab_url` | The HTTPS URL of the target GitLab repository (e.g., `https://gitlab.com/yourusername/yourrepo.git`).      | Yes      | N/A     |
| `username`   | Your GitLab username.                                                                                      | Yes      | N/A     |
| `gitlab_pat` | Your GitLab Personal Access Token. Must have permissions to write (`write_repository` scope) to the target repository. | Yes      | N/A     |
| `force_push` | Whether to force push to GitLab. If `true`, overwrites the destination branch. Use with caution.           | No       | `false` |

## Feedback and Contributions

Feedback, bug reports, and contributions are highly valued and welcome! Please feel free to:

-   Open an issue for any bugs or feature requests.
-   Submit a pull request with improvements.

If you find this action useful, please consider starring the repository!

## Disclaimer

This GitHub Action was developed to fulfill a specific synchronization need. While it has been tested, users should thoroughly verify its functionality and implications in their own environments before deployment to critical repositories. Use this action at your own discretion. The maintainers are not responsible for any data loss or unintended consequences.
