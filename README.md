# GH docs action workflow


## `./.github/workflows/hold-docs-prs.yml`

This workflow gets triggered when any pull request gets a new label.

The workflow checks if the label is named "pr: docs", and if so, adds a "pr: hold" label.

## `./.github/workflows/release.yml`

This workflow is triggered manually.
It takes two inputs:

- semver increment
- identifier

This is copy paste from `vercel/turborepo`'s `release.yml` workflow.

The workflow itself doesn't do anything, it just prints a success message. It exists only to show that other workflows can be triggered when it completes.

## `./.github/workflows/merge-docs-pr.yml`

This workflow is triggered when the Release workflow is _completed_.

The goal is to chain this workflow after a Release.

The name is a bit misleading, it does not actually _merge_ any PRs.

It looks for PRs with the label `pr: docs`, adds the `pr: automerge` label,
and removes the `pr: hold` label. The idea is that _other_ automation (like Kodiak)
will use these labels to merge the PR. (This is modeled this way, because turborepo
already uses Kodiak).

### TODO

Ideally, this workflow would read the triggering Release workflow's _inputs_
and use them to determine which docs PRs to merge.

The docs PRs themselves would need more granular labels to do this (e.g. `pr: docs minor` `pr: docs patch`, etc)
