# sync-branch
[![GitHub release (latest SemVer)](https://img.shields.io/github/v/release/jojomatik/sync-branch?sort=semver)](https://github.com/jojomatik/sync-branch/releases) [![GitHub](https://img.shields.io/github/license/jojomatik/sync-branch)](LICENSE) [![Build and publish](https://github.com/jojomatik/sync-branch/actions/workflows/publish.yml/badge.svg)](https://github.com/jojomatik/sync-branch/actions/workflows/publish.yml) 

Sync one branch to another

## Usage
```yml
name: Update another branch on each release
release:
  types:
    - published

jobs:
  sync_release_to_v1:
    if: ${{ startsWith(github.ref, 'refs/tags/v1') && !contains(github.ref, 'beta') }}
    name: Sync branch `main` to other branches (fast-forward enabled)
    runs-on: ubuntu-latest
    steps:
      - name: Keep `v1` up to date with `main` (fast-forward enabled)
        uses: jojomatik/sync-branch@v1
        with:
          # The branch to sync from
          #   Optional
          #   Default: github.ref_name
          source: "main"
          # The branch to sync to
          #   Optional
          #   Default: `beta`
          target: "v1"
          # The strategy to use, if fast-forward is not possible (merge, force, fail).
          #   Optional
          #   Default: `merge`
          strategy: "merge"
          # The name to create merge commits with
          #   Required, if strategy `merge` is used
          git_committer_name: ${{ secrets.BOT_GIT_NAME }}
          # The email to create merge commits with
          #   Required, if strategy `merge` is used
          git_committer_email: ${{ secrets.BOT_GIT_EMAIL }}
          # The access token to push to the repository
          #   Optional
          #   Default: github.ref_name
          github_token: ${{ secrets.GH_TOKEN }}
```

## Licensing
This project is licensed under the MIT License (MIT). See also [`LICENSE`](LICENSE).
