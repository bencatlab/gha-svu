# gha-svu

![publish](https://github.com/bencatlab/gha-svu/actions/workflows/publish.yml/badge.svg)

`gha-svu` is a GitHub Action that calculates semantic versions using [SVU v3+](https://github.com/caarlos0/svu).  
It provides an easy way to manage versioning for your projects based on semantic versioning principles.

## Features

- Automatically calculates the next semantic version (`next`, `major`, `minor`, `patch`, `prerelease`)
- Display the current version (`current`)
- Provides metadata and prerelease identifiers for versions
- Supports custom tagging modes and patterns (`all`, `light`, or `heavy`)
- Pin to a specific `svu` version or install the latest release
- Optionally pushes the calculated version tag to the remote repository


## Inputs

| Name              | Description                                                                 | Default   | Required |
|-------------------|-----------------------------------------------------------------------------|-----------|----------|
| `always`          | If no commits trigger a version change, increment the patch (`true/false`).| `false`   | No       |
| `command`         | SVU subcommand to run: `current`, `next`, `major`, `minor`, `patch`, `prerelease`. | `next`    | No       |
| `initial-version` | Initial version to use when no tags are present (`v0`).                    |           | No       |
| `log-directory`   | Only use commits that changed files in the given directories.              |           | No       |
| `metadata`        | Metadata to append to the version (e.g., `+build.42`).                     |           | No       |
| `prerelease`      | Prerelease identifier (e.g., `alpha`, `beta`).                             |           | No       |
| `push-tag`        | Push the new tag to the remote (`true/false`).                             | `false`   | No       |
| `svu-version`     | SVU version to install (e.g., `v3.2.3`). Leave blank to install the latest.|           | No       |
| `tag-mode`        | Which tags to consider: `all`, `light`, `heavy`.                          | `all`     | No       |
| `tag-pattern`     | Regex to ignore tags that do not match the given pattern.                  |           | No       |
| `tag-prefix`      | Tag prefix (e.g., `v`).                                                   | `v`       | No       |
| `verbose`         | Enable verbose SVU output (`true/false`).                                  | `false`   | No       |

## Outputs

| Name       | Description                                         |
|------------|-----------------------------------------------------|
| `changed`  | Whether the version has changed (`true/false`).     |
| `current`  | Current semantic version based on existing tags.    |
| `next`     | Next semantic version calculated by SVU.            |

## Usage

Below is an example of how to use this action in your GitHub workflow:

```yaml
name: Semantic Versioning

on:
  push:
    branches:
      - main

jobs:
  versioning:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Run gha-svu
        uses: bencatlab/gha-svu@v0.1.0
        with:
          command: 'next'
          push-tag: 'true'
          prerelease: 'beta'
          metadata: '+build.42'
          verbose: 'true'

      - name: Output version
        run: echo "Next version is ${{ steps.svu.outputs.next }}"
```

## Examples

### Example 1: Get the Current Version
To retrieve the current semantic version without modifying or pushing tags:
```yaml
with:
  command: 'current'
```

### Example 2: Increment the Version
To always increment the patch version even if no commits trigger a version change:
```yaml
with:
  always: 'true'
  command: 'next'
```

### Example 3: Use Custom Tagging
To restrict tags to a specific pattern and prefix:
```yaml
with:
  tag-pattern: '^v[0-9]+\\.[0-9]+\\.[0-9]+$'
  tag-prefix: 'v'
```

### Example 4: Push the Tag to Remote
To push the calculated version tag to the remote repository:
```yaml
with:
  push-tag: 'true'
```

## Contributing

Feel free to open issues or pull requests for bug reports, feature ideas, or improvements !

*Made with ❤️ by [bencatlab](https://github.com/bencatlab)*
