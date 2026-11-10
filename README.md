# nnex

Release automation for Swift libraries, CLI tools, macOS apps, and iOS apps

nnex guides Swift developers from project setup through release preparation, review, and publishing. It coordinates versioning, build and test commands, documentation, GitHub releases, and distribution for each target.

This repository contains release builds and release notes only. It does not contain nnex's source code.

## Install

Requires macOS 15 or later and Homebrew.

```sh
brew install nikolainobadi/nn/nnex
```

## Get started

Use nnex from your project's Git repository. Have the GitHub CLI (`gh`) installed and authenticated for GitHub publishing, along with the Swift or Xcode tools your project needs. App releases also require the appropriate Apple signing credentials and account access.

```sh
nnex init
```

Interactive setup detects targets and asks how to build, test, and distribute them. It saves project settings in `.nnex/nnex.json` and shared settings in `~/.nnex/nnex.json`. On first setup, you can choose a default release branch and an AI command, or choose no AI. For CLI tools and macOS apps, setup also collects Homebrew tap and public release repository settings; app targets have additional signing or App Store Connect setup.

Choose the release workflow for your target:

| Command | Publishes |
| --- | --- |
| `nnex ship package` | A Swift library as a tagged GitHub release |
| `nnex ship tool` | CLI binaries to a public release repo, with a Homebrew formula update |
| `nnex ship cask` | A signed, notarized macOS app to a public release repo, with a Homebrew cask update |
| `nnex ship app` | An iOS build to App Store Connect, with version and What's New updates and a tagged GitHub release |

For example, after configuring a CLI target, preview a patch release:

```sh
nnex ship tool patch --dry-run
```

The preview runs the configured build and test commands and reports the release plan without publishing. Add `--skip-build-and-test` if you want to omit those commands.

To prepare the release for review:

```sh
nnex ship tool patch --prepare-only
```

This stages documentation and release notes in `.nnex/pending` and stops after review. Once ready, resume without passing another version:

```sh
nnex ship tool
```

A normal release asks for confirmation before publishing. If several targets are configured, nnex prompts you to choose one; use `--target <name>` to select it explicitly.

## Release preparation

- Tool and Swift library releases can ship multiple skills and publish them to a marketplace together. Select skills in `nnex init`, or configure a target-level `skills` array in `.nnex/nnex.json`. Existing single-skill configurations continue to load without manual migration.
- When answering a missing skill setting on the command line, repeat `--skill` for each folder. Interactive releases for targets already shipping skills also offer newly discovered, unassigned skill folders; you can include them, skip them for this release, or stop offering them on later releases.
- Pass an exact version such as `1.2.0`, or a bump: `major`, `minor`, or `patch`. Omit the version to choose interactively.
- Use `--ai-version` to request a version suggestion from commit history when an earlier release and a working AI command are available.
- Configured AI skills can update documentation and public release READMEs. Their output is staged for review before publication.
- Use `--notes-file <path>` to supply release notes yourself. Without AI-generated or supplied notes, nnex stages a template for you to fill in during review.
- Use `--skip-docs` to skip AI documentation and release-note generation. Release notes are still required before publishing.
- Use `nnex ship app --testflight-only` to upload an iOS build without updating App Store versions or What's New.

Explore available options with the built-in help:

```sh
nnex --help
nnex ship tool --help
```

## Releases

Builds and release notes for every version are on the [releases page](https://github.com/nikolainobadi/nnex-releases/releases).
