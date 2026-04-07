# Reusable Check Pull Request Title

A reusable GitHub Actions workflow that validates pull request titles conform to the required semantic versioning prefix format.

## Purpose

This workflow enforces that every pull request title begins with a recognised version bump prefix. This convention allows other automation (such as release workflows) to determine the correct semver increment from the PR title without any additional input.

The workflow fails with a clear error message if the title is invalid, blocking the PR from merging until the title is corrected.

## Inputs

This workflow has no inputs. It reads the PR title directly from the GitHub API using the `pull_request` event context.

## Permissions

No additional permissions are required beyond the defaults. The workflow uses the built-in `GITHUB_TOKEN` in read-only mode to fetch the pull request details.

## PR title format

Titles must begin with one of the following prefixes (followed by a colon and a space):

| Prefix | Meaning |
|---|---|
| `patch:` | Backwards-compatible bug fix |
| `minor:` | Backwards-compatible new feature |
| `major:` | Breaking change |
| `rc-patch:` | Release candidate for a patch bump |
| `rc-minor:` | Release candidate for a minor bump |
| `rc-major:` | Release candidate for a major bump |

### Valid examples

```
patch: Fix typo in README
minor: Add support for new configuration option
major: Drop support for Node.js 18
rc-patch: Fix typo in README (release candidate)
rc-minor: Add support for new configuration option (release candidate)
rc-major: Drop support for Node.js 18 (release candidate)
```

## Calling this workflow

Create a workflow file in your repository that triggers on pull request events:

```yaml
name: Check Pull Request Title

on:
  pull_request:
    types:
      - opened
      - edited
      - synchronize
      - reopened

jobs:
  check-pr-title:
    uses: ac-on-ac/reusable-check-pull-request-title/.github/workflows/check-pr-title.yml@v1.0.0
```

> **Note:** The `pull_request` trigger is required. The workflow fetches the PR title via the GitHub API using `context.payload.pull_request.number`, which is only available on pull request events.

## Releases

This repository uses the [reusable-manual-release](https://github.com/ac-on-ac/reusable-manual-release) workflow to create releases. Releases are triggered manually from the **Actions** tab.
