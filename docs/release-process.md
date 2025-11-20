# Release Process

## Hotfix Release

# 1. Ensure you have all remote tags
git fetch --tags

# 2. Create the hotfix branch from v1.2.3 and switch to it
git checkout -b hotfix/v1.2.x v1.2.3

# 3. Push the new branch to GitHub and set up tracking
git push -u origin hotfix/v1.2.x