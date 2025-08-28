# Box ID Public Github Actions (OSS)

Public portion of the shared Box ID GitHub Actions &amp; Workflows which can be referenced by public repositories.

## Usage

To use the workflows in your repository, reference them directly in an action step through `using`.

<details>

<summary>Generic Example</summary>

```yaml
name: Example Workflow

on: [push]

jobs:
  example:
    runs-on: ubuntu-latest
    steps:
      - name: Run reusable action
        uses: box-id/github-actions-oss/.github/workflows/reusable_action@main
        with:
          input_1: value_1
```

</details>

## Elixir Workflows

### `elixir_version_bump.yml`

This workflow creates a PR for a version bump and collects a changelog for Elixir projects.

#### Requirements

- A file `./.github/changelog-config.json` must exist in the repository. This file can be copied and adjusted from
  [here](https://github.com/box-id/github-actions-oss/blob/main/.github/changelog-config.json)

**Note**: This workflow supports repositories at any stage:

- **With GitHub releases**: Uses the latest release as the baseline and increments based on PR labels
- **With git tags only**: Falls back to the latest version tag matching the configured prefix and increments based on PR
  labels
- **Fresh repositories**: Creates the first release as version 1.0.0 (ignores PR labels for the initial release)

#### Inputs

- `branch`: The branch to release from (default: `main`)
- `version-prefix`: The prefix to use for version tags (default: `v`)

#### Example Usage

```yaml
name: 👉 Trigger Version Release

on:
  workflow_dispatch:

jobs:
  create_version_bump_pr:
    name: Create Version Bump PR
    uses: box-id/github-actions-oss/.github/workflows/elixir_version_bump.yml@main
    with:
      version-prefix: "v" # optional, defaults to "v"
    secrets: inherit
```

### `elixir_publish_release.yml`

This workflow creates a GitHub release and optionally publishes an Elixir package to Hex.pm.

If the workflow detects no change between the version in `mix.exs` and the last-published GitHub release, it will
**not** create a release and publish to Hex.

#### Inputs

- `branch`: The branch to release from (default: `main`)
- `publish-to-hex`: Whether to publish the package to hex.pm (default: `false`)
- `elixir-version-file`: The Elixir version file (only for `publish-to-hex = true`, default: `.tool-versions`)
- `version-prefix`: The prefix to use for version tags (default: `v`)

#### Secrets

- `HEX_API_KEY`: Required when `publish-to-hex` is `true`

#### Example Usage

```yaml
name: 🔩 Publish Release (Auto)

on:
  push:
    branches:
      - main
    paths:
      - "mix.exs"

jobs:
  release:
    name: Publish a release
    uses: box-id/github-actions-oss/.github/workflows/elixir_publish_release.yml@main
    with:
      publish-to-hex: true
      version-prefix: "v" # optional, defaults to "v"
    secrets: inherit
```

## NPM Workflows

### `npm_version_bump.yml`

This workflow creates a PR for a version bump and collects a changelog.

#### Requirements

- A file `./.github/changelog-config.json` must exist in the repository. This file can be copied and adjusted from
  [here](https://github.com/box-id/github-actions-oss/blob/main/.github/changelog-config.json)

**Note**: This workflow supports repositories at any stage:

- **With GitHub releases**: Uses the latest release as the baseline and increments based on PR labels
- **With git tags only**: Falls back to the latest version tag matching the configured prefix and increments based on PR
  labels
- **Fresh repositories**: Creates the first release as version 1.0.0 (ignores PR labels for the initial release)

#### Inputs

- `branch`: The branch to release from (default: `main`)
- `version-prefix`: The prefix to use for version tags (default: `v`)

#### Example Usage

```yaml
name: 👉 Trigger Version Release

on:
  workflow_dispatch:

jobs:
  create_version_bump_pr:
    name: Create Version Bump PR
    uses: box-id/github-actions-oss/.github/workflows/npm_version_bump.yml@main
    with:
      version-prefix: "v" # optional, defaults to "v"
    secrets: inherit
```

### `npm_publish_release.yml`

This workflow creates a GitHub release and publishes the package to NPM.

If the workflow detects no change between the version in `package.json` and the last-published GitHub release, it will
**not** create a release and publish to NPM.

#### Inputs

- `branch`: The branch to release from (default: `main`)
- `version-prefix`: The prefix to use for version tags (default: `v`)

#### Example Usage

```yaml
name: 🔩 Publish Release (Auto)

on:
  push:
    branches:
      - main
    paths:
      - "package.json"

jobs:
  release:
    name: Publish a release
    uses: box-id/github-actions-oss/.github/workflows/npm_publish_release.yml@main
    with:
      version-prefix: "v" # optional, defaults to "v"
    secrets: inherit
```
