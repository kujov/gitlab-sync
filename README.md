# GitLab Sync - GitHub Action

**GitLab Sync** is a easy to use github action for syncing code to gitlab.

## My Motivation

I was sick of all the not working github actions so I just hacked one myself lol

## How to Use

To use this github action follow these steps below!

### Step 1: Set Up Secrets in GitHub


- `GITLAB_URL`: The URL of your GitLab repository (e.g., `https://gitlab.com/yourusername/yourrepo.git`).
- `USERNAME`: Your GitLab username.
- `GITLAB_PAT`: Your GitLab Personal Access Token.

### Step 2: Add the Workflow to Your GitHub Repository

Create a `.github/workflows/action.yml` file in your repository and add the following workflow configuration:

```yaml
name: Sync Repo to GitLab

on:
  push

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
    - uses: keninkujovic/gitlab-sync@v1
      with:
        gitlab_url: ${{ secrets.GITLAB_URL }}
        username: ${{ secrets.USERNAME }}
        gitlab_pat: ${{ secrets.GITLAB_PAT }}
```

### Step 3: Push Changes to GitHub

Now you just need to push to GitHub and all the changes will be synced to Gitlab!

## Inputs

Here are the required inputs

- `gitlab_url`: The URL of your GitLab repository.
- `username`: Your GitLab username.
- `gitlab_pat`: Your Personal Access Token for GitLab.

## Feedback and Contributions

Your feedback and contributions are welcome! Feel free to open an issue or submit a pull request in my repository.

Also if you like the actions star it!

## Disclaimer

This was built in need and not tested properly.
