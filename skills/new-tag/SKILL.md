---
name: new-tag
description: Bump version in package.json and push a new tag to both minion-mind and minion-mind-releases repos. Use when the user says "new tag", "bump version", "release new version", or wants to create a new release tag.
---

# New Tag Skill

Automate version bumping and tag creation for minion-mind releases.

## Usage

```
/new-tag             # Default: commit all changes + bump version + tag
/new-tag no-commit   # Only bump version and tag, don't commit pending changes
```

## Workflow

### Default mode (with commit)

1. Check for uncommitted changes with `git status`
2. If changes exist, show summary and commit all with descriptive message
3. Get current version from package.json
4. Increment patch version (e.g., 0.2.62 -> 0.2.63)
5. Update package.json with new version
6. Commit the version bump
7. Push to origin
8. Create and push tag to minion-mind repo
9. Create and push same tag to minion-mind-releases repo

### no-commit mode

Skip steps 1-2, only do version bump and tagging.

## Commands

```bash
# Check for uncommitted changes
git status --short

# Show change summary
git diff --stat

# Commit all changes (default mode)
git add -A
git commit -m "Description of changes

Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>"

# Get current version
grep '"version"' /Users/femtozheng/python-project/minion-mind/package.json

# Update version (replace OLD with NEW)
sed -i '' 's/"version": "OLD"/"version": "NEW"/' /Users/femtozheng/python-project/minion-mind/package.json

# Commit version bump
git add package.json
git commit -m "Bump version to X.Y.Z

Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>"
git push

# Create and push tag to minion-mind
git tag vX.Y.Z
git push origin vX.Y.Z

# Create and push tag to minion-mind-releases
cd /Users/femtozheng/python-project/minion-mind-releases
git pull
git tag vX.Y.Z
git push origin vX.Y.Z
```

## Notes

- Always pull latest changes before starting
- Tag format: `vX.Y.Z` (e.g., v0.2.63)
- The minion-mind-releases tag triggers GitHub Actions to build the release
- Default mode shows change summary before committing
