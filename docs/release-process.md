# Release Process

This document describes how releases are created and promoted, including standard releases, patch releases, and hotfixes.

## Standard Release

Standard releases are driven from the `main` branch and are created from pull requests that have been reviewed, approved, and merged. Code merged into `main` **must be in a production-ready state**.

- **Trigger**: Merge of a PR to `main` (i.e. a push to `main`).
- **Pipeline**: Build → deploy → test the commit on `main` created by the merged PR.
- **Beta tag**: If tests pass, a beta tag is created.
- **Promotion**: The beta tag can be manually promoted to a release tag by running the workflow against that specific beta tag.
- **Deployment**: The release tag can then be deployed to the desired environment.

## Patch Release

Use a patch release when you need to fix issues in an existing release *before* or *after* it is deployed to production, but you still want to base the fix off a specific released version.

- **Trigger**: Push to a `patch` branch.
- **Branch naming**: `patch/v1.2.x`, where `v1.2.x` matches the currently deployed production major/minor version.
- **Workflow**:
  - Create PRs targeting the `patch` branch.
  - Ensure the same fixes are also merged into `main`.
  - A push to the `patch` branch triggers the patch release process, which creates a new patch tag that can be deployed to any environment.

### Create Patch Branch

To create a patch branch, follow these steps (example uses `v1.2.3` → `patch/v1.2.x`):

1. Ensure you have all remote tags.
2. Create the patch branch from `v1.2.3` and switch to it.
3. Push the new branch to GitHub and set up tracking.

```bash
# 1. Ensure you have all remote tags
git fetch --tags

# 2. Create the patch branch from v1.2.3 and switch to it
git checkout -b patch/v1.2.x v1.2.3

# 3. Push the new branch to GitHub and set up tracking
git push -u origin patch/v1.2.x
```

## Hotfix Release

Use a hotfix release when you need to create a patch **from the currently deployed production version**, especially when a newer version exists but has not yet been deployed to production.

- **Trigger**: Push to a `hotfix` branch.
- **Branch naming**: `hotfix/v1.2.x`, where `v1.2.x` is the currently deployed production major/minor version.
- **Workflow**:
  - Create PRs targeting the `hotfix` branch.
  - Ensure all fixes are also merged into `main`.
  - A push to the `hotfix` branch triggers the hotfix release process, which creates a hotfix tag that can be deployed to any environment.

### Create Hotfix Branch

To create a hotfix branch, follow these steps (example uses `v1.2.3` → `hotfix/v1.2.x`):

1. Ensure you have all remote tags.
2. Create the hotfix branch from `v1.2.3` and switch to it.
3. Push the new branch to GitHub and set up tracking.

```bash
# 1. Ensure you have all remote tags
git fetch --tags

# 2. Create the hotfix branch from v1.2.3 and switch to it
git checkout -b hotfix/v1.2.x v1.2.3

# 3. Push the new branch to GitHub and set up tracking
git push -u origin hotfix/v1.2.x
```

### Workflow Summary

- **Standard release**: push to `main` → beta tag → promote to release tag → deploy.
- **Patch release**: branch `patch/vX.Y.x` from a specific release tag → push → patch tag → deploy.
- **Hotfix release**: branch `hotfix/vX.Y.x` from the currently deployed production version → push → hotfix tag → deploy.

