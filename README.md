# GitHub action to automatically merge the base branch into the current PR

[![Build Status](https://api.cirrus-ci.com/github/cirrus-actions/rebase.svg)](https://cirrus-ci.com/github/cirrus-actions/rebase) [![](https://images.microbadger.com/badges/version/cirrusactions/rebase.svg)](https://microbadger.com/images/cirrusactions/rebase) [![](https://images.microbadger.com/badges/image/cirrusactions/rebase.svg)](https://microbadger.com/images/cirrusactions/rebase)

After installation simply comment `/update` to trigger the action:


# Installation

To configure the action simply add the following lines to your `.github/workflows/rebase.yml` workflow file:

```yml
on: 
  issue_comment:
    types: [created]
name: Automatic Rebase
jobs:
  rebase:
    name: Rebase
    if: github.event.issue.pull_request != '' && contains(github.event.comment.body, '/rebase')
    runs-on: ubuntu-latest
    steps:
    - name: Checkout the latest code
      uses: actions/checkout@v2
      with:
        fetch-depth: 0
    - name: Automatic Sync
      uses: ankitvgupta/pr-updater@v1.4.0
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Restricting who can call the action

It's possible to use `author_association` field of a comment to restrict who can call the action and skip the rebase for others. Simply add the following expression to the `if` statement in your workflow file: `github.event.comment.author_association == 'MEMBER'`. See [documentation](https://developer.github.com/v4/enum/commentauthorassociation/) for a list of all available values of `author_association`.
