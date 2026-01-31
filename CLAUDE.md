# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an "awesome list" repository that curates and ranks open-source Python tooling projects written in other languages (primarily Rust). It uses the [best-of-generator](https://github.com/best-of-lists/best-of-generator) to automatically generate a ranked README from project metadata.

**Key insight**: This is NOT a code project. It's a metadata/content management repository where the README.md is auto-generated from `projects.yaml`.

## Architecture

```
projects.yaml          → Source of truth for all project metadata
     ↓
best-of-update-action  → GitHub Action that fetches metrics and generates README
     ↓
README.md              → Auto-generated ranked list (never edit directly)
```

**Template files:**

- `config/header.md` - README header template (supports variables: `{project_count}`, `{stars_count}`, `{category_count}`)
- `config/footer.md` - README footer template

## Working with This Repository

### Adding/Updating Projects

Edit `projects.yaml` only. Never modify README.md directly.

**Required project properties:**

- `name` - Unique project name
- `github_id` - Format: `owner/repo` (e.g., `astral-sh/uv`)

**Optional properties:**

- `category` - One of: `package-managers`, `linters`, `type-checkers`, `miscellaneous`
- `labels` - List of labels (e.g., `["written-in-rust"]`)
- `pypi_id`, `conda_id`, `npm_id`, `dockerhub_id`, `maven_id` - Package manager IDs

**Example entry:**

```yaml
- name: uv
  github_id: astral-sh/uv
  labels: ["written-in-rust"]
  category: "package-managers"
```

### Automated Updates

The GitHub workflow (`update-best-of-list.yml`) runs every Thursday at 6 PM UTC:

1. Creates an `update/YYYY.MM.DD` branch
2. Fetches GitHub metrics and package manager data
3. Regenerates README.md with updated rankings
4. Creates a PR and draft release

Manual trigger: Run workflow from GitHub Actions with optional version input.

### PR/Issue Guidelines

- Title format: `Add project: name` or `Update project: name`
- One project per PR/issue
- Use issue templates for suggestions

## Files You Should Know

| File                | Purpose                                      |
| ------------------- | -------------------------------------------- |
| `projects.yaml`     | All project metadata - the only file to edit |
| `config/header.md`  | README header template                       |
| `config/footer.md`  | README footer template                       |
| `latest-changes.md` | Auto-generated trending projects summary     |
| `history/`          | Historical snapshots of each update          |
